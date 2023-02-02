# Monitoring

## Honeybadger
We use [Honeybadger](https://www.honeybadger.io/) to monitor application exceptions and deployments. (Note: Here are some dlss specific instruction on how to install honeybadger)

Add the honeybadger gem to your existing repo. Bundle install the honeybadger gem in your repo, and require 'capistrano/honeybadger' in your Capfile.

Go to [honeybadger UI](https://app.honeybadger.io/projects) and click "Add a Project".

The [honeybadger docs](https://docs.honeybadger.io/ruby/integration-guides/rails-exception-tracking.html) contain instructions for how to set it up in a Rails application, but there is a wrinkle to consider as you do so. The instructions point out that installing will "[g]enerate a config/honeybadger.yml file. If you don't like config files, you can place your API key in the $HONEYBADGER_API_KEY environment variable."

If you decide on the former approach, you will want to set up your honeybadger config file in [sul-dlss/shared_configs](https://github.com/sul-dlss/shared_configs). If you would like to use environment variables instead, you will need some assistance from operations to encrypt your project's API key and set up the necessary environment variables via [sul-dlss/puppet](https://github.com/sul-dlss/puppet).

### Checkins

## Cron check-ins

Some projects that define cron jobs (configured via the whenever gem) are integrated with Honeybadger check-ins. These cron jobs will check-in with HB (via a curl request to an HB endpoint) whenever run. If a cron job does not check-in as expected, HB will alert.

Cron check-ins are typically configured in the following locations:
1. `config/schedule.rb`: This specifies which cron jobs check-in and what setting keys to use for the checkin key. See this file for more details.  It often defines a custom task that wraps running rake or rails runner along with the curl needed to perform the checkin.
2. `config/settings.yml`: Stubs out a Honeybadger check-in key for each cron job. Since we may not want to have a check-in for all environments, this stub key will be used and produce a null check-in.  The actual keys are created in the Honeybadger project when you create a specific check-in in the Checkin tab in HB.
3. `config/settings/production.yml` in shared_configs: This contains the actual check-in keys (per environment for any environment that should actually checkin, usually production).
4. HB notification page: Check-ins are configured per project in HB. To configure a check-in, the cron schedule will be needed, which can be found with `bundle exec whenever`. After a check-in is created, the check-in key will be available. (If the URL is `https://api.honeybadger.io/v1/check_in/rkIdpB` then the check-in key will be `rkIdpB`).

Some examples below:
- Schedule file:  https://github.com/sul-dlss/happy-heron/blob/main/config/schedule.rb
- HB Checkin Setup: https://app.honeybadger.io/projects/77112/check_ins
- HB keys configured: https://github.com/sul-dlss/shared_configs/blob/sul-h2-prod/config/settings/production.yml

### Slack Integrations

- In the Slack application go to the side bar under `Direct Messages` and click on the Honeybadger application. This should pop up a right side bar where you can click Settings. Then click on "Add Configuration". Then follow the instructions given.
- Set up a configuration with your PROD environment to the deploy channel.
- If you don't have any Environments showing, you will need to deploy your application to a VM via capistrano.

## OK Computer
If possible, rails-based projects should use [OK Computer](https://github.com/sportngin/okcomputer) to provide a status-check endpoint for their instances and their dependent services. This endpoint should have [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) enabled for those exposed status routes in its puppet file. Enabling CORS makes it possible for status checks (or any other CORS-enabled API) to be accessed from javascript, which allows multiple front-ends to be written for a given backend. [Searchworks-status](https://sul-dlss.github.io/searchworks-status/), for example, draws data from a number of SUL services' OK Computer endpoints at runtime to create a public dashboard.
