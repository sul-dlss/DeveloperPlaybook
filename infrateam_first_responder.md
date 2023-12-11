# Guide for Infrastructure Team First Responder

The DLSS Infrastructure team is using a rotating role of "first responder." This doc explains the concept of the first responder role and outlines specific duties and expectations.

## Specific Responsibilities
### Dependency Updates (once per week)

The first responder needs to make sure that all codebases needing updates have updates merged and deployed. Note that some projects may need to have PRs created by hand where automatic creation may have failed. It is helpful to post updates in the `#dlss-infrastructure` Slack channel to make sure the team is aware of this work, in case anyone is working in related codebases or looking to deploy changes.

#### Merge 'em

Run the `merge-all` script to automatically merge all dependency update PRs: https://github.com/sul-dlss/access-update-scripts/blob/master/merge-all.rb. Note that this script will only work with Ruby 2.6 or greater.  See the comments at the top for how to run and note you will need a github access token if you haven't previously created one.  Instructions for creating a token are here:  https://help.github.com/en/github/authenticating-to-github/creating-a-personal-access-token-for-the-command-line   Save your token somewhere secure for re-use since you won't be able to view it in the Github interface again.

It can be helpful to scan the list of PRs generated in Slack.  If you notice some projects don't have a PR even though the list of updated gems suggest they should, you can look at the log output of the dependecy update script: https://sul-ci-prod.stanford.edu/job/SUL-DLSS/job/access-update-scripts/job/master/  One potential problem would be an older unmerged or existing `update-dependencies` branch that prevented the creation of a new one by the script.

#### Deploy 'em

Use the `sdr-deploy` CLI, from your laptop, to deploy all infrastructure projects via capistrano to deployed environments: https://github.com/sul-dlss/sdr-deploy.

Note that you will need to be sure you can ssh into each of the VMs from your laptop. (See the [sdr-deploy README](https://github.com/sul-dlss/sdr-deploy/blob/main/README.md) for more about how to use the `check_ssh` command to do this.)

##### 1. Create a release tag

First, use `sdr-deploy` to create a release tag. This lets you deploy a known point in time without asking others to hold merges to `main` while deployments are in process. It also lets us rollback to a known good tag. (See the [sdr-deploy README](https://github.com/sul-dlss/sdr-deploy/blob/main/README.md) for more about how to use the `tag` command do this.)

##### 2. Deploy to stage

Then, **warn #dlss-infra-stage-qa-use** of the impending deployment to stage in case there is active testing going on; if so, be sure to either comment out that app or coordinate with tester and then deploy the tag you created above to stage using `sdr-deploy`.

##### 3. Run integration tests in stage

Then **run infrastructure-integration-tests** (see [documentation](#run-infrastructure-integration-tests) below) after deploy to stage.

We want the FR to ensure
  - dependency updates don't break cross-app functionality
  - this test suite remains useful

If some tests fail when running the whole test suite at once, but pass when run individually, that is ok -- as long as they each pass under some circumstances. Chances are not great that the full suite will pass in one go from a spotty off-campus connection.

If you're unsure whether a particular test failure indicates bad network luck, a regression in the application, or an out of date test, raise it for discussion (or ask another dev to retry from their laptop) in #dlss-infrastructure.

If on a Mac, you will get better results if you stay in the same "space" as the running tests (avoids focus issues with the browser).

##### 4. Deploy to prod

1. **Warn #dlss-infra-chg-mgmt** of the impending deployment to prod.

2. **Deploy the tag you created above to prod** using `sdr-deploy`.

  Note that the deployment script will
  - pull the latest repo content from github
  - check cocina-model versions for agreement
  - open the Nagios dashboard for the environment

##### 5. Deploy to QA

To complete the cycle, and ensure QA has the common environment, deploy there as well. Then, **warn #dlss-infra-stage-qa-use** of the impending deployment to QA in case there is active testing going on; if so, be sure to either skip that app or coordinate with tester and then deploy the tag you created above to QA using `sdr-deploy`.

### Notify Coverage for Following Week

Set Slack reminders in `#dlss-infrastructure` for next week's Monday morning. The reminders should indicate [who is first responder](https://docs.google.com/spreadsheets/d/13TJR93Yc9_eF5B7w4XDx6ggG_wb3aLkgCHjpLwmHPBA/) and who is on deck for that week, and should be set for 3 am Pacific time/6 am Eastern, so that the east coast early risers don't have to wait for it.
  * Documentation on Slack's `/remind` command:  https://get.slack.help/hc/en-us/articles/208423427-Set-a-reminder
  * E.g., if Alice and Bob are up next week, `/remind #dlss-infrastructure "@alice is the first responder week of January 8, and @bob is on deck" Monday at 3 am`

### Monitor Honeybadger (at least once per day)
Monitor [Honeybadger alerts](https://app.honeybadger.io/projects) for projects in the Infrastructure portfolio

### Monitor Slack Channels (at least once per day)
Slack channels relevant to applications in the portfolio:
  - `#dlme` - "Digital Library of the Middle East" - we are responsible for dlme-transform, related to indexing of materials.
  - `#dlss-aaas` - "Accessioning as a Service" - where accessioneers may surface problems
  - `#dlss-etds-dev` - Electronic Theses and Dissertations channel
  - `#dlss-infrastructure` <-- our team's main channel
  - `#dlss-preservation-dev`
  - `#dlss-sinopia-dev` - Sinopia is our linked data channel
  - `#earthworks` - for gis-robot-suite related discussion
  - `#sdr-operations`
  - `#sul-cap-collab` - has developers in the School of Medicine working on the Profiles project (which we connect to with our sul-pub system)
  - `#web-archiving`

### Check Your Email (at least once per day)
Address any issues from user feedback email lists, cron daemon emails, etc.

### Monitor PresCat Dashboard (at least once per week)
Anything appearing in red in the [PresCat Dashboard](https://preservation-catalog-web-prod-01.stanford.edu/dashboard) should be addressed. For guidance, see the [PresCat Wiki](https://github.com/sul-dlss/preservation_catalog/wiki).

### Monitor Honeybadger checkins (at least once per week)
All missing production checkins should be addressed. The following are current checkins:

* PresCat: https://app.honeybadger.io/projects/54415/check_ins
* Sul-Pub: https://app.honeybadger.io/projects/50046/check_ins
* H2: https://app.honeybadger.io/projects/77112/check_ins
* WAS Robot Suite: https://app.honeybadger.io/projects/51141/check_ins
* DSA: https://app.honeybadger.io/projects/50568/check_ins
* SDR API: https://app.honeybadger.io/projects/67994/check_ins
* Preassembly: https://app.honeybadger.io/projects/52900/check_ins
* Workflow: https://app.honeybadger.io/projects/58890/check_ins
* WAS Registrar App: https://app.honeybadger.io/projects/63547/check_ins
* Google Books: https://app.honeybadger.io/projects/67990/check_ins
* Hydra ETD: https://app.honeybadger.io/projects/55164/check_ins

Note that checkin updates are also sent to various slack channels.

## General Responsibilities
### Improve Troubleshooting Documentation as Needed

If you need to triage or troubleshoot a problem and realize some documentation is missing, please provide it. List of appropriate places:
* https://github.com/sul-dlss/DevOpsDocs - e.g., what an ops or devops person would need to know to handle the situation
* `README.md` or other top level markdown doc in codebase (viewable via github)
  * Which codebase would need to be apparent from the problem
* Wiki for the codebase
* ? - in general, consider where the person interested in the info might look first. An end-user might go to the wiki, a dev might go to the README, ops folks might head to DevOpsDocs. Use your best judgement and ask for feedback if unsure.

The above documents should be useful and current. Please submit improvements as PRs for review.

If for some reason documentation is a significant undertaking, the call for documentation can be filed as an issue and prioritized/resourced by management.

### Improve First Responder Instructions as Needed

We need this document to be useful and current.  Please submit improvements as PRs for review.

### Monitor Queues
Queue dashboards:
  - argo bulk action jobs (sidekiq)
    - https://argo.stanford.edu/queues/
  - robots (sidekiq)
    - https://robot-console-prod.stanford.edu
    - https://argo.stanford.edu/report/workflow_grid
  - pre-assembly (sidekiq)
    - https://sul-preassembly-prod.stanford.edu/queues
    - Note: failed Discovery Reports are generally okay, as they are dry runs for pre-assembly jobs.
  - preservation replication jobs (sidekiq)
    - https://preservation-catalog-web-prod-01.stanford.edu/queues/
  - dor-services-app (sidekiq, rabbitmq):
    - https://dor-services-prod.stanford.edu/queues
    - https://sul-rabbit-prod.stanford.edu/#/queues
      - for credentials, see https://github.com/sul-dlss/shared_configs/blob/dor-services-app-prod/config/settings/production.yml
  - dor-indexing-app (rabbitmq):
    - https://sul-rabbit-prod.stanford.edu/#/queues
      - for credentials, see https://github.com/sul-dlss/shared_configs/blob/dor-indexing-app-prod/config/settings/production.yml
  - google books (sidekiq)
    - https://sul-gbooks-prod.stanford.edu/queues
  - happy-heron (i.e. H2 self deposit)
    - rabbitmq: https://sul-rabbit-prod.stanford.edu/#/queues
      - for credentials, see https://github.com/sul-dlss/shared_configs/blob/sul-h2-prod/config/settings/production.yml
    - sidekiq: https://sdr.stanford.edu/queues
  - sdr-api (sidekiq)
    - https://sdr-api-prod.stanford.edu/queues
  - techmd service (sidekiq)
    - https://dor-techmd-prod-a.stanford.edu/queues
  - web-registrar-app (sidekiq)
    - https://was-registrar-app.stanford.edu/queues

### Monitor Nagios
Nagios alerts (https://sul-nagios-prod.stanford.edu/nagios/  The Services link in left column will give overview of checks.  The Problems->Services tab will give you what’s alerting.)
  * Production
    * [All services](https://sul-nagios-prod.stanford.edu/nagios/cgi-bin/status.cgi?hostgroup=infrastructure-prod&style=detail)
    * [Service alerts](https://sul-nagios-prod.stanford.edu/nagios/cgi-bin/status.cgi?hostgroup=infrastructure-prod&style=detail&servicestatustypes=28&hoststatustypes=15)
  * Stage
    * [All services](https://sul-nagios-stage.stanford.edu/nagios/cgi-bin/status.cgi?hostgroup=infrastructure-stage&style=detail)
    * [Service alerts](https://sul-nagios-stage.stanford.edu/nagios/cgi-bin/status.cgi?hostgroup=infrastructure-stage&style=detail&servicestatustypes=28&hoststatustypes=15)
  * QA
    * [All services](https://sul-nagios-stage.stanford.edu/nagios/cgi-bin/status.cgi?hostgroup=infrastructure-qa&style=detail)
    * [Service alerts](https://sul-nagios-stage.stanford.edu/nagios/cgi-bin/status.cgi?hostgroup=infrastructure-qa&style=detail&servicestatustypes=28&hoststatustypes=15)

### Monitor Grafana Dashboards
Grafana dashboards provided detailed statistics / instrumentation / performance monitoring.
* [Solr](https://sulstats.stanford.edu/d/sul-solr-overview/sul-solr?orgId=1)
* [DOR Services App DB](https://sulstats.stanford.edu/d/000000039/postgresql-database?orgId=1&refresh=10s&var-DS_PROMETHEUS=Prometheus&var-interval=$__auto_interval_interval&var-namespace=&var-release=&var-instance=dor-services-app-db-prod-a.stanford.edu:9187&var-datname=All&var-mode=All)
* [Workflow Service DB](https://sulstats.stanford.edu/d/000000039/postgresql-database?orgId=1&refresh=10s&var-DS_PROMETHEUS=Prometheus&var-interval=$__auto_interval_interval&var-namespace=&var-release=&var-instance=workflow-service-db-prod.stanford.edu:9187&var-datname=All&var-mode=All)
* [Apache](https://sulstats.stanford.edu/d/c558ecce-310d-465a-aa7d-787a74cc3cbd/apache-via-prometheus?orgId=1&refresh=1m): Various servers are available under "host".

### Check the logs
SDR logs are aggregated in AWS Cloudwatch. To view/search them, log into AWS Console as `ReadOnlyRole@sul-dlss-production` in `us-west-2`. Logs are in the following log groups:

* sdr-production
* sdr-stage
* sdr-qa

### Other Duties as Assigned
* Management may choose to have the first responder handle a non-project work ticket
  * If so, ensure you assign the ticket to yourself and put it in the "in progress" column of [the team's production priorities board](https://github.com/orgs/sul-dlss/projects/58)
* First responder may be asked to spearhead a work estimate https://github.com/sul-dlss-labs/estimation (note that these are, by definition, meant to be done by more than one person; if it's smaller, should it be a ticket in a project?)


## General
### First Responder Rotation Premises

* Respond to production issues in a timely fashion during business hours.
* Single process for handling questions outside the team's current work cycle.
* Share the responsibility for production outages across the team in a planned way.
* Every team member rotates through this responsibility.
* Encourage cross-training, since a first-responder will likely have to investigate applications with which they are unfamiliar.
* Shore up missing and outdated documentation of production code and processes for everyone (ops, devops, PSM, stakeholders, devs, etc), informed by actual attempts to find and use said documentation to investigate production issues.

### "First Responder" != "On Call"

The idiosyncratic name of the role is intentional. It is not the duty of the first responder to be on-call outside of the first responder's normal work hours (which may not line up exactly with business hours in Palo Alto). The first responder rotation is an effort to watch and triage production issues in an intentional and organized fashion, since no engineer was officially assigned this responsibility in the past, and such minding of things was haphazard.

### How to Triage Production Problems

If a user reports a problem, or if one is surfaced from monitoring, the first responder is meant to timebox an investigation of the problem (_TBD: no more than 30 min?_). It's fine to ask teammates with relevant expertise for help, but also think about what documentation is needed so the next person will need less help. It is _not_ the job of the first responder to fix the issue on the spot (though if the fix is trivial, it's fine to do so).

* All problems investigated should get a ticket UNLESS:
  * Investigation leads to discovery that there is no problem
    * If additional documentation would have made this clearer, then
      * First responder creates additional documentation (preferred) or a ticket for it
  * Fixing the problem will be as quick as writing up a ticket
    * First responder fixes the problem in this case.
* All new tickets should be added to the infrastructure team's [production priorities board](https://github.com/orgs/sul-dlss/projects/58) (select `Infrastructure Portfolio Production Priorities` from the "Projects" dropdown on the GitHub issue page)

### A note on prioritization

Prioritization is the responsibility of management, not the responsibility of the first responder. Though of course, if a developer becomes aware of what may be a high priority issue, and is unsure whether management is aware of the issue (or sufficiently aware of its severity), the developer is certainly encouraged to bring the issue to their manager's attention.

* Types of data to help with prioritization
  * Is this a significant production problem? A non-critical outage?
  * Is there a workaround?  If so, how onerous is it?  How adequately does it cover the unavailable or misbehaving functionality?
  * Does it affect lots of end-users?  A handful of super-users?
  * How many digital resources are affected?
  * Is it blocking time sensitive work?

### Should a first responder do work cycle work?
After completing the specific responsibilities listed above and any prioritized production issues, a developer may choose at their discretion to (1) perform some of the general responsibilities listed above; (2) work on other issues from the production priorities board; and/or (3) do work cycle work.

### What if first responder isn't available?

The idea is for the first responder to be "interruptible" for production problems during his/her week of coverage.  If there are significant blocks of time when this isn't true (e.g. ½ day meeting with no access to slack or email), or if life happens (illness, family emergency, etc.), hopefully the first responder can arrange for coverage.  ("I'll take one of your days if you can cover Tues for me").  Please notify `#dlss-infrastructure` Slack channel of changes.

When the first responder can't arrange for coverage, the default would be for the "on deck" responder to become first responder. The "on deck" person is the first responder for the following week.

### Swapping shifts
If you are not available for a shift, it is your responsibility to find a swap, not current first responder.
