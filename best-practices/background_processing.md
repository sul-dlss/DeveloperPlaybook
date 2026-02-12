# Background Processing at DLSS

There are several tools that provide background processing for Rails/Ruby-based applications. At DLSS,
we use [Sidekiq](http://sidekiq.org/), [Solid Queue](https://github.com/rails/solid_queue), and [Kicks (FKA Sneakers)](https://github.com/ruby-amqp/kicks).

## Sidekiq

Sidekiq relies on Redis for queue management and uses threads to run its jobs in the same process. This means that if there is any chance of your background jobs stepping on one another, you need to ensure that both your code and the gems that your code uses when processing background jobs are thread-safe. A common example of non-thread-safe code is `Dir.chdir` and the `Timeout` library. Avoid using these entirely.

Contributed Systems (ContribSys), the organization behind Sidekiq, offers paid support licenses (see `Sidekiq Pro` below), which provide enhanced features. However, the source code is hosted on [GitHub](https://github.com/mperham/sidekiq). The project also has a detailed [Wiki](https://github.com/mperham/sidekiq/wiki) page with links to best practices.

We use a mix of the [`capistrano-sidekiq`](https://github.com/seuros/capistrano-sidekiq) and [`dlss-capistrano`](https://github.com/sul-dlss/dlss-capistrano/#sidekiq-via-systemd) gems to manage Sidekiq processes on servers. 

If a machine with Sidekiq deployed to it via `capistrano-sidekiq` reboots, there will be no Sidekiq process without a developer to restart it, either through a deployment, or `bundle` command. To solve this problem, we have a `puppet` class to set up an init script for a system-level start on boot. It requires a `puppet` change, and some concurrency settings in an application `config/sidekiq.yml` file. For example, see [exhibits](https://github.com/sul-dlss/exhibits/blob/master/config/sidekiq.yml) and [puppet](https://github.com/sul-dlss/puppet/blob/production/hieradata/node/exhibits-prod-a.stanford.edu.eyaml#L10).

### Sidekiq Pro

Some of our apps require more background job resilience than others (e.g., dor-services-app, SDR robots, preservation_catalog). These applications use the paid Sidekiq Pro gem for one or both of these Pro-only features: [super fetch](https://github.com/sidekiq/sidekiq/wiki/Reliability#using-super_fetch), to prevent losing jobs that die if a VM goes down while Sidekiq workers are processing jobs; and [reliable push](https://github.com/sidekiq/sidekiq/wiki/Pro-Reliability-Client), to safeguard against network errors when pushing jobs to Redis.

To configure Sidekiq Pro, credentials must be present. See the [documentation](https://github.com/sul-dlss/common-accessioning/tree/main?tab=readme-ov-file#configuration) on how to set this up and where to get the credentials.

#### Subscription Renewal

Our subscription to Sidekiq Pro must be renewed every year (we pay one year at a time, not one month at a time). How does this work? Every February, ContribSys will send email to [`sul-devops-team@lists.stanford.edu`](https://mailman.stanford.edu/mailman/listinfo/sul-devops-team) with a magic link where payment method information can be put in such as `https://billing.stripe.com/p/subscription/recovery/live_YWNjdF8wSEpnRHRqb0oxbkJFNFFOUFNxeixfVHVkMWtjUDh5cWxwajFuamhoV3BFOVpEZ2IzWUhERQ0100tCoKveTm` (bogus link, just for example). The developers on the `sul-devops-team` list should all see this message, and as of Feb. 2026, this list has two folks from Ops, two folks from E&D Access, and four folks from E&D Infrastructure, to make sure we don't miss these messages. If the payment method is not updated within 6-7 days, the account will be cancelled, and that will cause problems with production deployments of applications that use Sidekiq Pro. Should this happen, it is simple to re-establish the subscription without having to change application credentials. To do so, you should head to the [ContribSys billing page](https://billing.contribsys.com/spro/) and click `Buy a new Sidekiq Pro subscription`. To link this up to the credentials used before, the key is to use the above `sul-devops-team` email address when filling out the new subscription form.

## Solid Queue

With newer applications (ca. 2024 on), we have tended to use the new Rails default background management gem, [Solid Queue](https://github.com/rails/solid_queue), which "supports delayed jobs, concurrency controls, recurring jobs, pausing queues, numeric priorities per job, priorities by queue order, and bulk enqueuing..." Solid Queue does not rely on Redis or RabbitMQ; its only dependency is a database, and our apps already have databases set up. Deployment of Solid Queue is managed by [dlss-capistrano](https://github.com/sul-dlss/dlss-capistrano#solidqueue-via-systemd).

## Kicks (FKA Sneakers)

Kicks is used for processing RabbitMQ queues. These queues are bound to exchanges where messages are published.
Remember you must remember to `ack` each job in your worker.

Kicks is used by dor-services-app, H3, and pre-assembly, and deployment is handled by [dlss-capistrano](https://github.com/sul-dlss/dlss-capistrano#sneakers-via-systemd).

## ActiveJob

[ActiveJob](http://guides.rubyonrails.org/active_job_basics.html) is a Rails framework for
declaring and running background jobs. It supports a variety of technologies, including Sidekiq.
ActiveJob is attractive, because it lets you switch out back ends with minimal impact 
on your application code.
