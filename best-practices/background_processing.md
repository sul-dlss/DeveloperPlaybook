# Background Processing at DLSS

There are many tools that provide background processing for Rails/Ruby-based applications. At DLSS,
we use [Delayed Job](https://github.com/collectiveidea/delayed_job),
[Resque](https://github.com/resque/resque), [Sneakers](https://github.com/jondot/sneakers) and [Sidekiq](http://sidekiq.org/). We don't have a set
requirement for which technology to use, but most of us seem to prefer Sidekiq.


## Resque

[Resque](https://github.com/resque/resque) is a Ruby application with a companion Sinatra-based
monitoring web app. It uses [Redis](http://redis.io) as its backend for managing the queues. Each queue is a
multivalued record (a List data structure) in Redis and Redis handles all the queueing issues like
concurrency, etc.

Resque workers poll one or more queues to get work. For each job they receive, they fork a process
to run the job using the self.perform(*args) method. You can run multiple workers per queue if you
want to have concurrent jobs for a queue. When a worker polls more than 1 queue, it does so in order
per job which you can leverage to implement priority queues.

Note that per the lead maintainer, Resque is currently
[in maintenance mode](http://resque.github.io/2016/03/10/resque-1.26.0-released.html).

The DOR Workflow Service framework uses Resque.


## Sidekiq

I believe Sidekiq was inspired by Resque, and is considered by most to improve on its ancestor. Like Resque, Sidekiq relies on Redis for queue management. Note that Sidekiq uses threads to run its jobs in the same process. This means that if there is any chance of your background jobs stepping on one another, you need to ensure that both your code and the gems that your code uses when processing background jobs are thread-safe. A common example of non-thread-safe code is `Dir.chdir` and the `Timeout` library. Avoid using these entirely.

It's fairly common to run Redis on its own server. If you do this, you need to know that Redis
doesn't have any notion of separate "accounts". The implication is that if several applications
share the same Redis server for ease of administration, they may step on each other and wipe out
each other's data if you're not careful. The
[redis-namespace gem](https://github.com/resque/redis-namespace) can help alleviate some of this
issue. There also seems to be a higher risk that you'll experience
[performance problems](https://redislabs.com/blog/benchmark-shared-vs-dedicated-redis-instances#.V7IfjrVrgUE)
when running shared Redis instances.

The organization behind Sidekiq offers paid support licenses, which also provide enhanced
features. However, the source code is hosted on [GitHub](https://github.com/mperham/sidekiq). The
project also has a nice [Wiki](https://github.com/mperham/sidekiq/wiki) page with links to best
practices etc. Overall I would say that the Sidekiq documentation is better than the Delayed Job
docs.

We use the [capistrano-sidekiq gem](https://github.com/seuros/capistrano-sidekiq) to let capistrano manage Sidekiq and sidekiq processes. If a machine with Sidekiq deployed to it reboots, there will be no sidekiq process without a developer to restart Sidekiq, either through a deployment, or `bundle` command. To solve this problem, we have a `puppet` class to set up an init script for a system level start on boot. It requires a `puppet` change, and that we manage concurrency settings in a Rails `config/sidekiq.yml` file. For example, see [exhibits](https://github.com/sul-dlss/exhibits/blob/master/config/sidekiq.yml) and [puppet](https://github.com/sul-dlss/puppet/blob/production/hieradata/node/exhibits-prod-a.stanford.edu.eyaml#L10).

The [DPN Synchronization](https://github.com/dpn-admin/dpn-sync) application uses Sidekiq.


## Sneakers

Sneakers is used for processing RabbitMQ queues. These queues are bound to exchanges where messages are published.
Remember you must remember to `ack` each job in your worker.

Sneakers is used by:
[dor_indexing_app](https://github.com/sul-dlss/dor_indexing_app/blob/e72cc7e01cc26f865a2f9cfdd52086df69ca0258/Gemfile#L25) 
[h2](https://github.com/sul-dlss/happy-heron/blob/c0ffd3f06a4f33c3099394067b6da809929c7856/Gemfile#L72)


## Delayed Job

Like Sidekiq, Delayed Job supports [ActiveJob](http://guides.rubyonrails.org/active_job_basics.html).

[This article](https://www.sitepoint.com/delayed-jobs-best-practices/) suggests some best practices
for Delayed Job.

[sul_pub](https://github.com/sul-dlss/sul_pub/blob/16525c3083ba5494428397168ca6de8d011e7b5e/Gemfile#L26) uses Delayed Job (with Active Job, see below).


## Active Job

[Active Job](http://guides.rubyonrails.org/active_job_basics.html) is a Rails framework for
declaring and running background jobs. It supports a variety of technologies, including Delayed Job
and Sidekiq. Active Job is attractive, because it lets you switch out back ends with minimal impact
on your application code. Note, however, that Active Job does not currently have a mechanism for
detecting when a background job has failed. Most of the actual processing libraries (Delayed Job,
Sidekiq etc.) have facilities for recognizing when a job has failed, though, so if this is important
to you, it may be necessary to go without the layer of convenience and consistency that Active Job
provides.


## Deployment

[Capistrano::Sidekiq](https://github.com/seuros/capistrano-sidekiq) and
[Capistrano::DelayedJob](https://github.com/AgileConsultingLLC/capistrano3-delayed-job) provide
specific Capistrano tasks for Sidekiq and Delayed Job respectively.
