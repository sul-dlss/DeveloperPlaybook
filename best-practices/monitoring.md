# Monitoring

## Honeybadger
We use [Honeybadger](https://www.honeybadger.io/) to monitor application exceptions and deployments. (Note: Here are some dlss specific instruction on how to install honeybadger)

Add the honeybadger gem to your existing repo. Bundle install the honeybadger gem in your repo, and require 'capistrano/honeybadger' in your Capfile. 

Go to [honeybadger UI](https://app.honeybadger.io/projects) and click "Add a Project".

The [honeybadger docs](https://docs.honeybadger.io/ruby/integration-guides/rails-exception-tracking.html) contain instructions for how to set it up in a Rails application, but there is a wrinkle to consider as you do so. The instructions point out that installing will "[g]enerate a config/honeybadger.yml file. If you don't like config files, you can place your API key in the $HONEYBADGER_API_KEY environment variable."

If you decide on the former approach, you will want to set up your honeybadger config file in [sul-dlss/shared_configs](https://github.com/sul-dlss/shared_configs). If you would like to use environment variables instead, you will need some assistance from operations to encrypt your project's API key and set up the necessary environment variables via [sul-dlss/puppet](https://github.com/sul-dlss/puppet).

### Slack Integrations

- In the Slack application go to the side bar under `Direct Messages` and click on the Honeybadger application. This should pop up a right side bar where you can click Settings. Then click on "Add Configuration". Then follow the instructions given.
- Set up a configuration with your PROD environment to the deploy channel.
- If you don't have any Environments showing, you will need to deploy your application to a VM via capistrano.
