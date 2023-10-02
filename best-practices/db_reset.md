# Resetting the db for a deployed Rails application

Unfortunately:
* A simple `rails db:reset` will not work to reset the db for a deployed application.
* Servers are not consistent, so no single process will work in all cases. 

However, the following should handle most cases:
1. Stop all services connecting to the db. This can be accomplished by executing the following as ksu on each of the application's servers:
```
systemctl stop apache2
systemctl stop passenger
systemctl stop sneakers
systemctl stop sidekiq-*
```
Note: An application may only have some of these services. There is no harm in executing the command if a service is absent.

2. Drop the database: `RAILS_ENV=production bundle exec rails db:drop` (If you really mean it, precede this command with: `DISABLE_DATABASE_ENVIRONMENT_CHECK=1`)

Note:
* Rails may complain that there remaining db connections. If so, look for any processes that may be connecting to the db with `ps -fu <APP USER>` and stop the processes.
* The user might not have permissions to drop the database. In that case, connect with psql as the postgres user (`psql -U postgres`) and drop the database: `DROP DATABASE <DB NAME>;`
* If a peer authentication error occurs when connecting with psql as the postgres user then:
```
su postgres
psql
```

3. Create the database by running puppet on the db server as ksu: `puppet agent --test --environment=production`. This will create the database with the appropriate settings (as specified in puppet).

Note:
* The db server may be local (i.e., the same as the server running the Rails application) or a separate, dedicated server.
* If local, execute the following before running puppet: `source /etc/profile.d/rvm.sh`

4. Migrate: `RAILS_ENV=production bundle exec rails db:migrate`

5. Seed if required by the application: `RAILS_ENV=production bundle exec rails db:seed`
