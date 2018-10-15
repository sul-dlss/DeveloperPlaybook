# Updating Ruby

Ruby can be updated on our deployment servers by updating the version in a Puppet hieradata file:

```diff
role::params::app_ruby:
-  'ruby-2.4.4':
+  'ruby-2.5.1':
    default_use: true
```

Before rolling this change out to the `production` branch of Puppet, you can manually install the Ruby version on a server and its bundler dependencies to prevent any downtime.

```sh
# On server
[appuser@server]$ ksu
# Install new Ruby verison
[root@server]$ rvm install ruby-2.5.1
# As app user run a bundle install
[appuser@server]$ cd app/current
[appuser@server]$ rvm use 2.5.1
[appuser@server]$ bundle install
```

After this is done in advance, the server is ready for the Ruby version Puppet changes with hopefully no downtime.

** Note: ** Any Puppet managed crontab entries may need an update.
