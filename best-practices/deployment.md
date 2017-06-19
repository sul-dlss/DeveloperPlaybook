## Ruby Applications

We use [`capistrano`](http://capistranorb.com) to deploy our applications to one or more machines. Capistrano gives us a common workflow for deploying our applications and is shipped with reasonable defaults for a wide variety of Rails, Sinatra, and other Ruby-based applications. It can also be used for deploying non-Ruby applications. By providing a consistent way to deploy an application, we make it easier for everyone to contribute to and maintain our applications.

The provided [capistrano documentation(http://capistranorb.com/)] is excellent.  We also have many projects at https://github.com/sul-dlss that use capistrano and can be referred to as exemplars.

We maintain [`dlss-capistrano`](https://github.com/sul-dlss/dlss-capistrano), a distribution of Capistrano plugins that all our deployed applications should take advantage of. The gem includes:

- `capistrano/one_time_key`, for integrating Capistrano with our kerberos-based environment;
- `capistrano/bundle_audit`, for checking applications for known security vulnerabilities before launching.
- `capistrano/shared_configs`, for getting the latest relevant files from our shared_configs repo as part of deployment.

Other common plugins (not bundled in `dlss-capistrano`) include:

- `capistrano/honeybadger`, for reporting deploys to Honeybadger service
- `capistrano-passenger`, for integrating deploys with Passenger web server
- `capistrano-sidekiq`, for integrating deploys with Sidekiq background jobs
- `whenever/capistrano`, for integrating deploys with whenever cron jobs

### Hostnames and Usernames

We do not consider hostnames and application usernames as "sensitive" information, so they may be present in capistrano deploy files in a public github repo.

### Stage names

Preferred names for Capistrano "stage" files are:

- `dev`  (i.e. `config/deploy/dev.rb`)
- `stage` (i.e. `config/deploy/stage.rb`)
- `prod` (i.e. `config/deploy/prod.rb`)

### Literal strings for servers and users

DevOps runs reports that depend on the explicit literal strings for the server's fully qualified domain name and for the user being present in the specific capistrano "stage" file.  For example:

##### GOOD

```ruby
# https://github.com/sul-dlss/exhibits/blob/master/config/deploy/dev.rb#L2
server 'exhibits-dev.stanford.edu', user: 'exhibits', roles: %w(web db app)
```

##### BAD

Do not do this in capistrano "stage" file nor in deploy.rb either
```ruby
set :deploy_host, 'my-box-prod.stanford.edu'
set :user, 'macro_hamster'
server fetch(:deploy_host), user: fetch(:user), roles: %w(web db app)
```

### Literal string for deploy_to

##### GOOD

```ruby
set :deploy_to, '/opt/app/was/was-registrar'
```

##### BAD

```ruby
set :user, 'macro_hamster'
set :deploy_to, '/opt/app/%{fetch(:user)}/%{fetch(:application)}'
```

### Do not lock Capistrano gem version

##### BAD

```ruby
# config valid only for current version of Capistrano
lock '3.8.0'
```
