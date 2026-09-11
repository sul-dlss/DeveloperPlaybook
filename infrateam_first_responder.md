# Guide for Infrastructure Team First Responder

The DLSS Infrastructure team is using a rotating role of "first responder." This doc explains the concept of the first responder role and outlines specific duties and expectations.

## Specific Responsibilities
### Dependency Updates (once per week)

The first responder needs to make sure that all codebases needing updates have updates merged and deployed. Note that some projects may need to have PRs created by hand where automatic creation may have failed. It is helpful to post updates in the `#dlss-infrastructure` Slack channel to make sure the team is aware of this work, in case anyone is working in related codebases or looking to deploy changes.

#### Merge 'em

Run the `merge-all` script to automatically merge all dependency update PRs: https://github.com/sul-dlss/access-update-scripts/blob/main/merge-all.rb. Note that this script will only work with Ruby 2.6 or greater.  See the comments at the top for how to run and note you will need a github access token if you haven't previously created one.  Instructions for creating a token are here:  https://help.github.com/en/github/authenticating-to-github/creating-a-personal-access-token-for-the-command-line   Save your token somewhere secure for re-use since you won't be able to view it in the Github interface again.

It can be helpful to scan the list of PRs generated in Slack.  If you notice some projects don't have a PR even though the list of updated gems suggest they should, you can look at the log output of the dependecy update script: https://sul-ci-prod.stanford.edu/job/SUL-DLSS/job/access-update-scripts/job/main/  One potential problem would be an older unmerged or existing `update-dependencies` branch that prevented the creation of a new one by the script.

NOTE: Should any of the builds fail due to errors updating the CircleCI ruby-rails orb version, see the `Jenkins CircleCI Integration` section below

#### Deploy 'em

Use the `sdr-deploy` CLI to deploy all infrastructure portfolio projects to deployed environments. This CLI can be used either on the `sdr-infra` server, or from your laptop (if you've set up SSH with multiplexing and proxy jump per [playbook documentation of common SSH configuration](/best-practices/ssh_configuration.md)). See the [`sdr-deploy` README](https://github.com/sul-dlss/sdr-deploy) for more about how to use the CLI for mass deploys, and how to use the CLI's `check_ssh` command to verify access to all of the VMs in a given environment.

##### 1. Create a release tag

First, use `sdr-deploy` to create a release tag. This lets you deploy a known point in time without asking others to hold merges to `main` while deployments are in process. It also lets us rollback to a known good tag. (See the [sdr-deploy README](https://github.com/sul-dlss/sdr-deploy/blob/main/README.md) for more about how to use the `tag` command do this.)

##### 2. Deploy to stage

Then, **warn #dlss-infra-stage-qa-use** of the impending deployment to stage in case there is active testing going on; if so, be sure to either comment out that app or coordinate with tester and then deploy the tag you created above to stage using `sdr-deploy`.

##### 3. Run integration tests in stage

Then **run infrastructure-integration-tests** after deploy to stage.

Run with:

```
bin/rspec
```

See https://github.com/sul-dlss/infrastructure-integration-test/blob/main/README.md for more info.

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
  - open the Zabbix dashboard for the environment

NOTE: The `sul-dlss/rialto-airflow` project has a check (via a capistrano task) to stop the deploy if any DAG is currently running.  This is because the deploy would restart docker and thus kill the running DAG (which we do not want).   This is more likely to happen in production. If this happens, it will look like the deploy has failed (which it has, on purpose).  However, the sdr-deploy deployment script will not show you the reason.  To verify the deployment failed because the DAG was running, you can clone the repo separately (if you don't have it installed already) and then try to deploy it on its own.  The error message will be clearly shown in the capistrano output.  If the reason for the failure is that the DAG is running, you can safely skip the dependency update deploy and let it occur the next time a deployment is done.

##### 5. Deploy to QA

To complete the cycle, and ensure QA has the common environment, deploy there as well. Then, **warn #dlss-infra-stage-qa-use** of the impending deployment to QA in case there is active testing going on; if so, be sure to either skip that app or coordinate with tester and then deploy the tag you created above to QA using `sdr-deploy`.

##### 6. Deploy to AWS

The [speech-to-text](https://github.com/sul-dlss/speech-to-text) project is deployed automatically to qa and stage environments in AWS when a version is tagged `rel-YYYY-MM-DD`. If the integration tests runs clean then you can trigger a production deploy by manually [creating a release](https://github.com/sul-dlss/speech-to-text/releases/new) in Github, using the `rel-YYYY-MM-DD` tag.

### Notify Coverage for Following Week

Set Slack reminders in `#dlss-infrastructure` for next week's Monday morning. The reminders should indicate [who is first responder](https://docs.google.com/spreadsheets/d/13TJR93Yc9_eF5B7w4XDx6ggG_wb3aLkgCHjpLwmHPBA/) and who is on deck for that week, and should be set for 3 am Pacific time/6 am Eastern, so that the east coast early risers don't have to wait for it.
  * Documentation on Slack's `/remind` command:  https://get.slack.help/hc/en-us/articles/208423427-Set-a-reminder
  * E.g., if Alice and Bob are up next week,
      ```
      /remind #dlss-infrastructure "@alice is the first responder week of January 8, and @bob is on deck" Monday at 3 am
      ```

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
* WAS Robot Suite: https://app.honeybadger.io/projects/51141/check_ins
* DSA: https://app.honeybadger.io/projects/50568/check_ins
* SDR API: https://app.honeybadger.io/projects/67994/check_ins
* Preassembly: https://app.honeybadger.io/projects/52900/check_ins
* Workflow: https://app.honeybadger.io/projects/58890/check_ins
* WAS Registrar App: https://app.honeybadger.io/projects/63547/check_ins
* Google Books: https://app.honeybadger.io/projects/67990/check_ins
* Hydra ETD: https://app.honeybadger.io/projects/55164/check_ins

Note that checkin updates are also sent to various slack channels.

### Jenkins CircleCI Integration

Infrastructure team projects use CircleCI for continuous integration, and the vast majority of these projects use a [CircleCI Orb](https://circleci.com/developer/orbs/orb/sul-dlss/ruby-rails) the team manages to run Ruby / Rails tests. Weekly dependency updates include checking for new versions of the orb and bumping the version in the project's CircleCI configuration. When the weekly updates run, Jenkins uses an access token stored in Vault to interact with the CircleCI API. Should you need to know or change these values, they are located at:

* puppet/application/jenkins/circle_token
  * NOTE: This entry includes both a `content` value (the token itself) and an `expiry_date` value, since the max life for a CircleCI access token is one year.
* puppet/application/jenkins/circle_login
* puppet/application/jenkins/circle_passwd
* puppet/application/jenkins/circle_recovery_code
  * NOTE: You'll need to use this recovery code in lieu of a 2FA code, at which point CircleCI will generate a new recovery code. Make sure to update vault with this new value!

##### Renewing the Access Token (once per year)

In advance of the access token aging out...

1. Log in to CircleCI using the Vault credentials above. Note the new recovery code and be sure to update this value in Vault!
2. Generate a new access token at https://app.circleci.com/settings/user/tokens
3. Set the name of the new token to something like `access-update-scripts` (it doesn't much matter), and set the expiry date for a year after creation. 
4. Update the `circle_token` entry in Vault, both the `content` value and the `expiry_date` value.
5. Set a reminder to renew the token a week or so in advance of the next access token expiry date:
    ```
    /remind #dlss-infrastructure "@infrastructure-devs :firstresponder: it's time to generate a new CircleCI access token for Jenkins! see https://github.com/sul-dlss/DeveloperPlaybook/blob/main/infrateam_first_responder.md#jenkins-circleci-integration for details" August 24th, 2027
    ```

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
  - google books (sidekiq)
    - https://sul-gbooks-prod.stanford.edu/queues
  - heracles-etd
    - mission_control: https://etd.stanford.edu/jobs
  - hungry-hungry-hippo (i.e. self deposit)
    - rabbitmq: https://sul-rabbit-prod.stanford.edu/#/queues
      - for credentials, see https://github.com/sul-dlss/shared_configs/blob/sul-h3-prod/config/settings/production.yml
    - mission_control: https://sdr.stanford.edu/jobs
  - sdr-api (sidekiq)
    - https://sdr-api-prod.stanford.edu/queues
  - techmd service (sidekiq)
    - https://dor-techmd-prod-a.stanford.edu/queues
  - web-registrar-app (sidekiq)
    - https://was-registrar-app.stanford.edu/queues

### Monitor Zabbix
[Zabbix](https://dlss-zabbix.stanford.edu/zabbix.php?action=dashboard.list) provides monitoring of servers. (This is a replacement for Nagios.)
  * Production: https://dlss-zabbix.stanford.edu/zabbix.php?action=dashboard.view&dashboardid=397
  * Stage: https://dlss-zabbix.stanford.edu/zabbix.php?action=dashboard.view&dashboardid=399
  * QA: https://dlss-zabbix.stanford.edu/zabbix.php?action=dashboard.view&dashboardid=398

### Check the logs
SDR logs are aggregated in AWS Cloudwatch. To view/search them, log into AWS Console as `ReadOnlyRole@sul-dlss-production` in `us-west-2`. Logs are in the following log groups:

* sdr-production
* sdr-stage
* sdr-qa

### Other Duties as Assigned
* Management may choose to have the first responder handle a non-project work ticket
  * If so, ensure you assign the ticket to yourself and put it in the "in progress" column of [the team's production priorities board](https://github.com/orgs/sul-dlss/projects/58)
* First responder may be asked to spearhead a work estimate https://github.com/sul-dlss-labs/estimation (note that these are, by definition, meant to be done by more than one person; if it's smaller, should it be a ticket in a project?)

## Processes
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
