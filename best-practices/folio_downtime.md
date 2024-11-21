# FOLIO Downtime and SDR

When FOLIO needs to be taken down wholly or in part, for maintenance or Fiscal Year Rollover (FYRO), SDR applications can be configured to reduce calls to FOLIO's APIs. 

## FOLIO Downtime
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

`bundle exec cap sidekiq_systemd:restart`

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
* Quiet the relevant processes which monitor queues for jobs that update FOLIO. Quieting queues means that they finish any in-flight jobs and don't pick up any more jobs. Use the Sidekiq UI to quiet the queue by using the "Pause" button.
  * releaseWF_update-marc_dsa queue: https://robot-console-prod.stanford.edu/busy
  * ETD process monitoring the submit_marc queue. https://etd.stanford.edu/queues/busy

### During rollover
* Monitor #folio-implementation for updates. 
* Monitor Honeybadger

### After rollover completed
Restart the process on dor-services-app and hydra_etd:

`bundle exec cap sidekiq_systemd:restart`

Monitor Sidekiq, Honeybadger, and the Argo workflow grid.
