# New Project "To Production" Checklist

Feel free to make a copy of this checklist when beginning work on a new development project. You may or may not wish to store the copy for posterity, e.g., as part of the [project's DevOpsDocs](https://github.com/sul-dlss/DevOpsDocs).

## Application

- [ ] deploy via [Capistrano](http://capistranorb.com/) (using [dlss-capistrano](https://github.com/sul-dlss/dlss-capistrano) gem)
- [ ] CI via [TravisCI.org](https://travis-ci.org/organizations/sul-dlss/repositories), [TravisCI.com](https://travis-ci.com/organizations/sul-dlss/repositories), or [CircleCI](https://app.circleci.com/projects/project-dashboard/github/sul-dlss).
- [ ] code coverage by tests via [Coveralls](https://coveralls.io/github/sul-dlss) or [CodeClimate](https://codeclimate.com/oss/dashboard).
- [ ] badges in repo’s README for above tools
- [ ] config files via [`shared_configs`](https://github.com/sul-dlss/shared_configs)
  - [ ] deploy gets latest config files via `capistrano/shared_configs` (part of `dlss-capistrano`; examples in stacks and purl)
- [ ] exceptions to be handled by [HoneyBadger](https://app.honeybadger.io/projects)
  - [ ] deploy info sent to honeybadger (examples in stacks and purl deploy.rb)
- [ ] (Rails) setup [`okcomputer`](https://github.com/sportngin/okcomputer)
   - set up at 'status' endpoint [example](https://github.com/sul-dlss/stacks/blob/master/config/initializers/okcomputer.rb#L6-L7)
   - naming conventions for okcomputer checks
     - 'external-xxx', e.g. 'external-solr', when target is external to VM;
     - 'feature-xxx' when target is part of VM or app code
   - settings.yml and other config .yml files may indicate which dependencies to check
   - optional checks are those that shouldn't take a VM out of a load balancer, but are "nice to know at a glance"
- [ ] determine the appropriate nagios monitoring checks (audit existing checks if applicable) and make it so.
- [ ] document regularly scheduled maintenance windows for the application / dependencies
- [ ] determine log retention policy and make it so.
- [ ] write operations-concerns doc
- [ ] Google Analytics + Webmaster Tools
     - [ ] Apply [IP address anonymization configuration](https://support.google.com/analytics/answer/2763052?hl=en)
- [ ] determine SLA levels
  - prod:
  - stage:
  - dev:
  NOTE:
  - high SLA alerts -- need immediate attention from on-call staff. 24x7 coverage.
  - medium SLA alerts -- urgent response needed from on-call staff, but it can wait until the morning. 10x7 coverage.
  - low SLA alerts -- response needed from on-call staff, but it can wait until NBD if necessary. 10X5 coverage.
  - none - email to dev list. Not the responsibility of on-call staff, but may still require action.
- [ ] mailing list for administrators / maintainers (replace_with_xxx@lists.stanford.edu)
- [ ] long term user advocate (replace_with_sunetid)

## Puppet Facts

- [ ] set `project` (replace_me)
- [ ] set appropriate `sla_level`  (prod: xxx; stage: yyy; dev: zzz)
- [ ] set `technical_team` (replace_with_techteam)
- [ ] set `github_url` (https://github.com/sul-dlss/REPLACE_ME)
- [ ] set `user_advocate` (replace_with_sunetid)
- [ ] set `operations_concerns_url` (https://github.com/sul-dlss/DevOpsDocs/blob/master/projects/REPLACE_ME)
- [ ] set `configs_branch_name` (prod: xxx; stage: xxx; dev: xxx)

## Nagios

- [ ] monitor (Rails) `/status/all` (okcomputer) or `/is_it_working`, or other, plus any other appropriate checks
    - [ ] high SLA alerts -- need immediate attention from on-call staff. 24x7 coverage.
    - [ ] medium SLA alerts -- urgent response needed from on-call staff, but it can wait until the morning. 10x7 coverage.
    - [ ] low SLA alerts -- response needed from on-call staff, but it can wait until NBD if necessary. 10X5 coverage.
    - [ ] none - email to dev list. Not the responsibility of on-call staff, but may still require action.
    - [ ] are there "soft dependency" notifications for a user advocate
          (e.g. SearchWorks advocate learns Requests is down, so no SW user can make requests)
- [ ] ensure monitoring is happening on new nagios only
- [ ] add url to this checklist as the notes url field
- [ ] if load balanced, nagios check for load balancer url

## System(s)

- [ ] new puppetized
- [ ] upgrade system dependencies
    - [ ] Ruby (using supported version)
    - [ ] passenger
    - [ ] yum packages (if/when applicable)
- [ ] request certificates for application (HTTPS)
- [ ] load balanced (if applicable)
    - [ ] health check monitors (Rails) `/status/all` (okcomputer) or `/is_it_working` (or other) for _HTTP response code_
    - [ ] configured to not allow traffic directly to servers if outside Stanford network
- [ ] load tested & passenger threads configured accordingly
- [ ] passenger threads pre-warmed to fqdn (on single app vms)
- [ ] service names served by DNS alias’ rather than server names
- [ ] development, staging, demo servers should not be accessible from off campus
- [ ] request WebAuthGeneral directory access for service (if applicable; used for workgroup-based authentication)
