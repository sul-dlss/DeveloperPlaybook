# Guide for Infrastructure Team First Responder

The DLSS Infrastructure team is using a rotating role of "first responder." This doc explains the concept of the first responder role and outlines specific duties and expectations.

## Table of Contents

* ["First Responder" Rotation Premises](#first-responder-rotation-premises)
  * ["First Responder" != "On Call"](#first-responder--on-call)
* [Duties](#duties)
  * [Weekly Dependency Updates](#weekly-dependency-updates)
    * [Merge 'em](#merge-em)
    * [Deploy 'em](#deploy-em)
      * [Important Exceptions](#important-exceptions)
      * [Code that isn't a Ruby Application](#code-that-isnt-a-ruby-application)
  * [Verify / Notify Coverage for Following Week](#verify--notify-coverage-for-following-week)
  * [Sign Up for Your Next First Responder Shift](#sign-up-for-your-next-first-responder-shift)
  * [Run infrastructure-integration-tests](#run-infrastructure-integration-tests)
  * [Proactively Check for Production Problems](#proactively-check-for-production-problems)
  * [Triage Production Problems](#triage-production-problems)
  * [Improve Troubleshooting Documentation as Needed](#improve-troubleshooting-documentation-as-needed)
  * [Improve First Responder Instructions as Needed](#improve-first-responder-instructions-as-needed)
  * [Other Duties as Assigned](#other-duties-as-assigned)
* [How to Proactively Check for Production Problems](#how-to-proactively-check-for-production-problems)
* [How to Triage Production Problems](#how-to-triage-production-problems)
  * [A note on prioritization](#a-note-on-prioritization)
* [What if first responder isn't available?](#what-if-first-responder-isnt-available)

## "First Responder" Rotation Premises

* Respond to production issues in a timely fashion during business hours.
* Single process for handling questions outside the team's current work cycle.
* Share the responsibility for production outages across the team in a planned way.
* Every team member rotates through this responsibility.
* Encourage cross-training, since a first-responder will likely have to investigate applications with which they are unfamiliar.
* Shore up missing and outdated documentation of production code and processes for everyone (ops, devops, PSM, stakeholders, devs, etc), informed by actual attempts to find and use said documentation to investigate production issues.

### "First Responder" != "On Call"

The idiosyncratic name of the role is intentional. It is not the duty of the first responder to be on-call outside of the first responder's normal work hours (which may not line up exactly with business hours in Palo Alto). The first responder rotation is an effort to watch and triage production issues in an intentional and organized fashion, since no engineer was officially assigned this responsibility in the past, and such minding of things was haphazard.

# Duties

## Weekly Dependency Updates

The first responder needs to make sure that all codebases needing updates have updates merged and deployed. Note that some projects may need to have PRs created by hand where automatic creation may have failed. It is helpful to post updates in the `#dlss-infrastructure` Slack channel to make sure the team is aware of this work, in case anyone is working in related codebases or looking to deploy changes.

### Merge 'em

Run the `merge-all` script to automatically merge all dependency update PRs: https://github.com/sul-dlss/access-update-scripts/blob/master/merge-all.rb. Note that this script will only work with Ruby 2.6 or greater.  See the comments at the top for how to run and note you will need a github access token if you haven't previously created one.  Instructions for creating a token are here:  https://help.github.com/en/github/authenticating-to-github/creating-a-personal-access-token-for-the-command-line   Save your token somewhere secure for re-use since you won't be able to view it in the Github interface again.

### Run infrastructure-integration-tests

We want the FR to ensure
  - dependency updates don't break cross-app functionality
  - this test suite remains useful

To that end, all the infrastructure-integration-tests must be run successfully in the qa and/or stage environment before deploying to production (one of qa or stage is sufficient;  testing in environments helps ensure our environments are (still) set up properly.).  This should be done as part of deployment of dependency updates as described in the next section.

If some tests fail when running the whole test suite at once, but pass when run individually, that is ok -- as long as they each pass under some circumstances. Chances are not great that the full suite will pass in one go from a spotty off-campus connection.

If you're unsure whether a particular test failure indicates bad network luck, a regression in the application, or an out of date test, raise it for discussion (or ask another dev to retry from their laptop) in #dlss-infrastructure.

If on a Mac, you will get better results if you stay in the same "space" as the running tests (avoids focus issues with the browser).

### Deploy 'em

Use the `sdr-deploy` script to deploy all infrastructure projects (with **required additional deploys** noted below) via capistrano to deployed environments: https://github.com/sul-dlss/sdr-deploy

Note that you will need to be sure you can ssh into each of the VMs from wherever you are running the deploy script.

- **stage**: deploy to stage with script, then **run infrastructure-integration-tests** after deploy to stage (and|or qa).  
  - before deploying, **warn #dlss-infra-stage-use** in case there is active testing going on;  be sure to either comment out that app or coordinate with tester
- **qa**: deploy to qa with script
- **prod**: finally, deploy to prod with script

Note that the deployment script will attempt to verify the status check URL for projects that have one and will report success or failure for each project.
There are currently two projects which cannot be verified in production since those servers are locked down and do not allow external requests (even on full tunnel VPN).
These projects are `suri_rails` and `workflow-server-rails`.  To verify deployment, you can visit Honeybadger and curl from the servers:

Honeybadger:
- Workflow server rails: https://app.honeybadger.io/projects/58890/deploys
- Suri : https://app.honeybadger.io/projects/70269/deploys

Status check from the server (ssh into the prod server for that project and then use curl):
- Workflow server rails: `curl -i https://workflow-service-prod.stanford.edu/status/all`
- Suri: `curl -i https://sul-suri-prod.stanford.edu/status/all`

### Required Additional Deploys

There are applications that need to be deployed separately (i.e., not using `sdr-deploy`).

#### Ruby Projects with non-standard Capistrano targets

* **hydra_etd `uat` environment**: deploy via `cap uat deploy` in `hydra-etd`
* **sul-pub `cap-dev` enviroment**: deploy via `cap cap-dev deploy` in `sul_pub` (note all [ENV values](https://github.com/sul-dlss/sul_pub/tree/main/config/deploy))

#### Cloud Projects
As of August 2021, deployment of cloud projects (Sinopia and DLME) happens via CircleCI, not manually.  You will need to make releases, however.

* **Sinopia apps**:  Deployed when a new release is created. FR ACTION NEEDED: the sinopia_editor project requires bumping the version number and a couple other steps before tagging a new release, see Sinopia [Release Process](https://github.com/LD4P/sinopia_editor/blob/main/release_process.md) for details.
* **dlme-transform**: Check that [CircleCI](https://app.circleci.com/pipelines/github/sul-dlss/dlme-transform) published the latest image after dependency updates were merged. This image is pulled afresh for every data transform. More details in [README](https://github.com/sul-dlss/dlme-transform/#deploying) and [DevOpsDocs](https://github.com/sul-dlss/DevOpsDocs/blob/master/projects/dlme/operations-concerns.md#deployment-info).

### Code that isn't a Ruby Application

We have codebases that aren't Ruby applications or gems. We have not yet settled on a long-term method for dealing with these:

* java code
  * the Infrastructure team is has the following Java repositories in our portfolio, none of which are actively maintained by the team
    * [dlss-wowza](https://github.com/sul-dlss/dlss-wowza/)
    * [etdpdf](https://github.com/sul-dlss/etdpdf)
    * [openwayback](https://github.com/sul-dlss/openwayback)
    * [wasapi-downloader](https://github.com/sul-dlss/wasapi-downloader)
    * [WASMetadataExtractor](https://github.com/sul-dlss/WASMetadataExtractor)

We currently do not have an automatic update mechanism for our Java projects.

Note that security updates affecting our Ruby **gems** will be caught when doing capistrano deployments via `gemfile audit`.  For some projects, we've also enabled a GitHub setting that allows GH to automatically create pull requests (via `Dependabot`) to address security vulnerabilities (for merge by human reviewers).

## Verify / Notify Coverage for Following Week

1. Verify first responder for following week is still able to cover it. Check this with on deck person on Monday (and keep in mind throughout the week, e.g., if person becomes ill).  (schedule: https://docs.google.com/spreadsheets/u/1/d/13TJR93Yc9_eF5B7w4XDx6ggG_wb3aLkgCHjpLwmHPBA/)
  1. It's the scheduled responder's responsibility to find a swap, not current first responder.
  1. The person covering the following week is "on deck" for this week.
1. Set Slack reminders in `#dlss-infrastructure` for next week's Monday morning. The reminders should indicate who is first responder and who is on deck for that week, and should be set for 3 am Pacific time/6 am Eastern, so that the east coast early risers don't have to wait for it.
  * Documentation on Slack's `/remind` command:  https://get.slack.help/hc/en-us/articles/208423427-Set-a-reminder
    * E.g., if Alice and Bob are up next week, `/remind #dlss-infrastructure "@alice is the first responder week of Monthuary 8, and @bob is on deck" Monday at 3 am`

## Sign Up for Your Next First Responder Shift

The infrastructure team has 9 developers, so you should be taking a shift every 9 or so weeks.
* Schedule:  https://docs.google.com/spreadsheets/u/1/d/13TJR93Yc9_eF5B7w4XDx6ggG_wb3aLkgCHjpLwmHPBA/

## Proactively Check for Production Problems

See [How to Proactively Check for Production Problems](#how-to-proactively-check-for-production-problems) section below for specifics.

## Triage Production Problems

If a user reports a problem, or if one is surfaced from monitoring, the first responder ought to timebox an investigation of the problem (_TBD: 30 min?_). See [How to Triage Production Problems](#how-to-triage-production-problems) section below for more specifics.

## Improve Troubleshooting Documentation as Needed

If you need to triage or troubleshoot a problem and realize some documentation is missing, please provide it. List of appropriate places:
* https://github.com/sul-dlss/DevOpsDocs - e.g., what an ops or devops person would need to know to handle the situation
* README or other top level markdown doc in codebase (viewable via github)
  * Which codebase would need to be apparent from the problem
* Wiki for the codebase
* ? - in general, consider where the person interested in the info might look first. An end-user might go to the wiki, a dev might go to the README, ops folks might head to DevOpsDocs. Use your best judgement and ask for feedback if unsure.

The above documents should be useful and current. Please submit improvements as PRs for review.

If for some reason documentation is a significant undertaking, the call for documentation can be filed as an issue and prioritized/resourced by management.

## Improve First Responder Instructions as Needed

We need this document to be useful and current.  Please submit improvements as PRs for review.

## Other Duties as Assigned

* Management may choose to have the first responder handle a non-project work ticket
  * If so, ensure you assign the ticket to yourself and put it in the "in progress" column of [the team's production priorities board](https://github.com/orgs/sul-dlss/projects/37)
* First responder may be asked to spearhead a work estimate https://github.com/sul-dlss-labs/estimation (note that these are, by definition, meant to be done by more than one person; if it's smaller, should it be a ticket in a project?)

# How to Proactively Check for Production Problems

At the very least, the first responder should be watching:

* [Honeybadger alerts](https://app.honeybadger.io/projects) for projects in the Infrastructure portfolio
  * **Note** that you may want to resolve old alerts in the Honeybadger UI at the beginning of your first responder shift so you have a clean slate to monitor.
* Feedback email lists, e.g. argo-feedback, sinopia-feedback, etc
* Slack channels relevant to applications in the portfolio:
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
* Queue dashboards:
  - robots
    - https://robot-console-prod.stanford.edu/overview
      - https://robot-console-prod.stanford.edu/failed
    - https://argo.stanford.edu/report/workflow_grid
  - pre-assembly
    - https://sul-preassembly-prod.stanford.edu/resque/overview
  - preservation
    - https://preservation-catalog-web-prod-01.stanford.edu/resque/overview
      - https://preservation-catalog-web-prod-01.stanford.edu/resque/failed
        - When debugging pres_cat errors: https://github.com/sul-dlss/preservation_catalog/wiki/Investigating-failed-Resque-Jobs
  - web-registrar-app
    - https://was-registrar-app.stanford.edu/queues
  - dor-services-app sidekiq (for jobs run by workers in other threads):
    - https://dor-services-prod.stanford.edu/queues/
  - google books
    - https://sul-gbooks-prod.stanford.edu/queues
  - sdr-api
    - https://sdr-api-prod.stanford.edu/queues
  - techmd service
    - https://dor-techmd-prod.stanford.edu/queues
* cron daemon emails
* GitHub security alerts

As a loose guideline, we recommend checking each of these at least once or twice a day to stay on top of potential issues.

Other places to monitor:
* Nagios alerts (https://sul-nagios-prod.stanford.edu/nagios/  The Services tab on the left column will give overview of checks.  The Problems->Services tab will give you what’s alerting.)
* Grafana at https://sulstats.stanford.edu/ gives different visualizations of running
infrastructure servers with the option to create customized dashboards.
* If you hear about possible security issues through other avenues (e.g. `#iso-public` on Slack, your consumption of the news in general), and you feel that the issues may be relevant and uncaptured, file an issue and call attention to it as needed.

# How to Triage Production Problems

If a user reports a problem, or if one is surfaced from monitoring, the first responder is meant to timebox an investigation of the problem (_TBD: no more than 30 min?_). It's fine to ask teammates with relevant expertise for help, but also think about what documentation is needed so the next person will need less help. It is _not_ the job of the first responder to fix the issue on the spot (though if the fix is trivial, it's fine to do so).

* All problems investigated should get a ticket UNLESS:
  * Investigation leads to discovery that there is no problem
    * If additional documentation would have made this clearer, then
      * First responder creates additional documentation (preferred) or a ticket for it
  * Fixing the problem will be as quick as writing up a ticket
    * First responder fixes the problem in this case.
* All new tickets should be added to the infrastructure team's [production priorities board](https://github.com/orgs/sul-dlss/projects/37) (select `Infrastructure Portfolio Production Priorities` from the "Projects" dropdown on the GitHub issue page)

## A note on prioritization

Prioritization is the responsibility of management, not the responsibility of the first responder. Though of course, if a developer becomes aware of what may be a high priority issue, and is unsure whether management is aware of the issue (or sufficiently aware of its severity), the developer is certainly encouraged to bring the issue to their manager's attention.

* Types of data to help with prioritization
  * Is this a significant production problem? A non-critical outage?
  * Is there a workaround?  If so, how onerous is it?  How adequately does it cover the unavailable or misbehaving functionality?
  * Does it affect lots of end-users?  A handful of super-users?
  * How many digital resources are affected?
  * Is it blocking time sensitive work?

# What if first responder isn't available?

The idea is for the first responder to be "interruptible" for production problems during his/her week of coverage.  If there are significant blocks of time when this isn't true (e.g. ½ day meeting with no access to slack or email), or if life happens (illness, family emergency, etc.), hopefully the first responder can arrange for coverage.  ("I'll take one of your days if you can cover Tues for me").  Please notify `#dlss-infrastructure` Slack channel of changes.

When the first responder can't arrange for coverage, the default would be for the "on deck" responder to become first responder. The "on deck" person is the first responder for the following week.
