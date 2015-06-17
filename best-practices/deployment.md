## Ruby Applications

We use [`capistrano`](http://capistranorb.com) to reliably deploy our applications to one or more machines. Capistrano gives us a common workflow for deploying our applications and is shipped with reasonable defaults for a wide variety of Rails, Sinatra, and other Ruby-based applications. By providing a consistent way to deploy an application, we make it easier for everyone to contribute to and maintain our applications.

We maintain [`lyberteam-capistrano-devel`](https://github.com/sul-dlss/lyberteam-capistrano-devel), a distribution of Capistrano plugins that all our deployed applications should take advantage of. The gem includes:

- `capistrano/one_time_key`, for integrating Capistrano with our kerberos-based environment;
- `capistrano/releaseboard`, for reporting deploys to a common releaseboard application;
- `capistrano/bundle_audit`, for checking applications for known security vulnerabilities before launching.

Other common plugins (not yet bundled in `lyberteam-capistrano-devel`) include:

- `squash/rails/capistrano3`, for reporting deploys to our Squash exception handling application

We avoid putting hostnames, users, and other "sensitive" information in the capistrano deploy configuration. Instead, we may prompt the deployer for that information:

```ruby
# https://github.com/sul-dlss/earthworks/blob/master/config/deploy/production.rb#L3
set :deploy_host, ask("Server", 'e.g. server.stanford.edu')

# https://github.com/sul-dlss/exhibits-request/blob/master/config/deploy/production.rb
require 'dotenv'
Dotenv.load
# ...
set :deploy_host, ENV['CAPISTRANO_PRODUCTION_DEPLOY_HOST'] || ask(:deploy_host, "")
```

We use different Capistrano environments for each environment we deploy to. Commonly used names for these Capistrano environments include:

- `development`
- `staging`
- `production`
