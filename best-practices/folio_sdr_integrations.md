# FOLIO and SDR Integrations

## FOLIO Upgrades

When Folio is upgraded, SDR integrations should be tested.  This is typically done as a FOLIO upgrade in -stage to allow for this testing.

The following are some tests that can be done to verify the SDR integrations continue to work as expected.  It is not necessarily exhaustive, but will cover the major touch points.

Note: it is possible that the data in Folio stage may not match the data in Argo stage, which will lead to barcodes/instance HRIDs in argo stage that are not found in Folio stage.  This may be because the data in Folio stage was replaced with a snapshot of Folio prod data.  This may mean you will need to ask for help in #sdr-operations or #libsys-infra-folio-integration to find example objects to work with (or trying looking in Argo prod for relevant objects).

### Infra Apps Using Folio

As of Sep 2025, the following apps connect to Folio.  See https://github.com/search?q=org%3Asul-dlss%20gem%20%27folio_client%27&type=code

They use the folio-client gem: https://github.com/sul-dlss/folio_client

- DSA - connects to Folio to fetch MARC records (to refresh descriptive metadata or look up metadata by barcode/key), and write to MARC records (when released to Searchworks for example).
- Google Books - connects to Folio to fetch marc records when harvesting google books
- Rialto-orgs - connects to Folio during scheduled data harvests (every ~2 weeks) to pull information about a users' primary department affiliation
- Argo - connects to Folio to verify a catalog record exists when a user enters a barcode/key during registration
- Hydra ETD - connects to Folio to write a stub MARC record and then check if an ETD is fully cataloged before accessioning.
- Heracles ETD - connects to Folio to write a stub MARC record and then check if an ETD is fully cataloged before accessioning.

### Integration Tests

Run the following infrastructure integration tests and ensure they pass:

- spec/features/item_creation_with_folio_hrid_spec.rb
- spec/features/sdr_deposit_spec.rb
- spec/features/etd_creation_spec.rb

### Searchworks Release

1. Find an item in Argo stage with a Folio Instance HRID, so that it can be released.  Here is one example: https://argo-stage.stanford.edu/view/druid:bc930hq4019, ask in #sdr-operations or #libsys-infra-folio-integration for others.
2. Go to Folio stage (folio-stage.stanford.edu) (presuming this is the environment that was upgraded and you are testing) and find the record via the Folio Instance HRID by using the "Inventory" search feature in Folio.  Check to see if it has PURL URL already listed in the record details.  It will be under a heading called "Electronic Access" as a resource (you will need to scroll down to find this section).  If a URL exists, the item has already been released...if not, it has not been released.  A non-released item is best.
3. Back in Argo, use the "Manage release" button to release the item to Searchworks.  Verify that the job completes successfully and then on the item details page, verify that the releaseWF completed successfully.
4. Back in Folio stage, refresh the item details and verify the electronic resource PURL now appears in the record.
5. Reverse the process by using the "Manage release" button in Argo to unrelease the items in Searchworks.  The process should happen again, this time removing the "Electronic Access" entry from Folio.

### Google Books Barcode Lookup

1. Find an item with a barcode in Argo stage (see also the note above about how data may not match with Folio stage).  https://argo-stage.stanford.edu/catalog?all=true&f%5Bidentifiers%5D%5B%5D=has_barcode
2. Go to the google-books stage VM and on the rails console, lookup the record for this barcode, e.g.

`FolioClient.fetch_marc_xml(barcode: '36105123784501')`

You should get an XML record back.

### ETD Catalog Status Check

The ETD app sends records to Folio, and then pulls metadata back from Folio when a cataloger completes the process on their end.  The integration tests above will verify information going to Folio, this will verify the status check that comes back from Folio.

1. Run the ETD infrastructure integration test to get a druid for an ETD (or use the one from above if you just ran that test).
2. Find this item in Argo stage to get the folio instance HRID.  Verify the etdSubmitWF is not complete (it will have some steps at the end marked as waiting).
3. Find this record in Folio stage and verify the "Instance status term" under the "Administrative Data" section in the record details is "Uncataloged".
4. Ask libsys team (via the #libsys-infra-folio-integration) to update this record in Folio so that it is cataloged.  This will update the status in the Folio record to "Cataloged".
5. Either wait an hour (for the ETD cron job to run) or run the task manually on the ETD VM: `bin/rails runner -e production "CatalogStatus.run"`  Note there may be some errors for missing druids if the data on etd-stage and folio-stage is not fully in sync (which it may not be).
6. Return to Argo stage and look at the item again.  Verify the etdSubmitWF has now completed.

## FOLIO Downtime

When FOLIO needs to be taken down wholly or in part, for maintenance or Fiscal Year Rollover (FYRO), SDR applications can be configured to reduce calls to FOLIO's APIs.

When all of FOLIO (or its Okapi API gateway) are taken down for maintenance, SDR applications will not be able to reach FOLIO. During downtime, avoid operations that write to FOLIO. Read operations will fail during downtime and should retry. This includes:
* Refreshing metadata via Argo
* Argo registration that looks up an HRID
* ETD CatalogStatus cron that checks FOLIO for cataloged ETDs.

### Before downtime
* google-books: Go to Settings to pause retrieving MARCXML. Google Books does barcode lookups in FOLIO to get metadata.
* Quiet job queues for workflow steps that write to FOLIO. Quieting queues means that they finish any in-flight jobs and don't pick up any more jobs. Use the Sidekiq UI and the "Quiet" button for relevant queues:
  * releaseWF_update-marc_dsa queue: https://robot-console-prod.stanford.edu/busy
  * ETD process monitoring the submit_marc queue. https://etd.stanford.edu/queues/queues

### During downtime
* Monitor #folio-implementation and #libsys-infra-folio-integration for updates.
* Monitor Honeybadger.

### After downtime
* Restart the processes on dor-services-app and hydra_etd:

`bundle exec cap <env> sidekiq_systemd:restart`

* Turn Google Books retrieval setting back on.
* Monitor Sidekiq, Honeybadger, and the Argo workflow grid.
* If the downtime was on stage/qa and involved upgrading FOLIO, consider running integration tests that interact with FOLIO:
  * spec/features/item_creation_with_folio_hrid_spec.rb
  * spec/features/sdr_deposit_spec.rb


## FOLIO FY Rollover (FYRO)

### Requirements
During rollover, do not run workflow steps that update FOLIO records, since FOLIO data may need to be wiped/restored as part of rollover. FOLIO itself, including Okapi, is usually up during this time.

Relevant workflow steps:
* etdSubmitWF
  * submit-marc: creates a stub marc record and submits it to FOLIO.
  * check-marc: cron job to check whether cataloger has fully cataloged ETD. This can still run while rollover happens, because all it does is look for is a status change. It does not change any FOLIO record.
  * catalog-status: once the ETD is cataloged fully, the ETD can be accessioned and released to SW. This WF is OK to run since it does not change FOLIO records.
* releaseWF
  * releaseWF_update-marc_dsa: adds/updates MARC 856 links to purl

hydra_etd has a submit_marc queue for CreateStubMarcRecordJob, with its own process.


### Before the rollover
* Quiet the relevant processes which monitor queues for jobs that update FOLIO. Quieting queues means that they finish any in-flight jobs and don't pick up any more jobs. Use the Sidekiq UI "Quiet" button to quiet the queue.
  * releaseWF_update-marc_dsa queue: https://robot-console-prod.stanford.edu/busy
  * ETD process monitoring the submit_marc queue. https://etd.stanford.edu/queues/busy

### During rollover
* Monitor #folio-implementation for updates.
* Monitor Honeybadger

### After rollover completed
Restart the process on dor-services-app and hydra_etd:

`bundle exec cap sidekiq_systemd:restart`

Monitor Sidekiq, Honeybadger, and the Argo workflow grid.
