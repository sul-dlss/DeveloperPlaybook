## Ruby Applications

We use [`capistrano`](http://capistranorb.com) to reliably deploy our applications to one or more machines. Capistrano gives us a common workflow for deploying our applications and is shipped with reasonable defaults for a wide variety of Rails, Sinatra, and other Ruby-based applications. By providing a consistent way to deploy an application, we make it easier for everyone to contribute to and maintain our applications.

We maintain [`dlss-capistrano`](https://github.com/sul-dlss/dlss-capistrano), a distribution of Capistrano plugins that all our deployed applications should take advantage of. The gem includes:

- `capistrano/one_time_key`, for integrating Capistrano with our kerberos-based environment;
- `capistrano/bundle_audit`, for checking applications for known security vulnerabilities before launching.

Other common plugins (not yet bundled in `dlss-capistrano`) include:

- `squash/rails/capistrano3`, for reporting deploys to our Squash exception handling application, or
- `capistrano/honeybadger`, for reporting deploys to Honeybadger

We do not consider hostnames and users as "sensitive" information in the capistrano deploy configuration. For ease of deploy, add this information into the deploy configuration.

```ruby
# https://github.com/sul-dlss/exhibits/blob/master/config/deploy/staging.rb#L2
server 'exhibits-stage.stanford.edu', user: 'exhibits', roles: %w(web db app)
```

We use different Capistrano environments for each environment we deploy to. These should match the suffix of the hostname for the server(s) it is being deployed to, which will most likely already use the shorthand environment name (i.e. `dev` over `development`). Commonly used names for these Capistrano environments include:

- `dev`
- `stage`
- `prod`
