# Background Processing at DLSS

There are many tools that provide background processing for Rails/Ruby-based applications. At DLSS,
we use [Delayed Job](https://github.com/collectiveidea/delayed_job),
[Resque](https://github.com/resque/resque) and [Sidekiq](http://sidekiq.org/). We don't have a set
requirement for which technology to use, but most of us seem to prefer Sidekiq.


## Delayed Job

Delayed Job is usually considered a little easier to get started with than Sidekiq, since it allows
you to use your (Rails) application's pre-existing database for storing the job queue. Like Sidekiq,
Delayed Job supports [ActiveJob](http://guides.rubyonrails.org/active_job_basics.html).

[This article](https://www.sitepoint.com/delayed-jobs-best-practices/) suggests some best practices
for Delayed Job.


## Sidekiq

I believe Sidekiq was inspired by Resque, and is considered by most to improve on its ancestor. As
opposed to Delayed Job, both of these tools require [Redis](http://redis.io) for queueing
jobs. Note that Sidekiq uses threads to run its jobs in the same process. This means that if there
is any chance of your background jobs stepping on one another, you need to ensure that both your
code and the gems that your code uses when processing background jobs are thread safe.

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


## Active Job

[Active Job](http://guides.rubyonrails.org/active_job_basics.html) is a Rails framework for
declaring and running background jobs. It supports a variety of technologies, including Delayed Job
and Sidekiq. Active Job is attractive, because it lets you switch out back ends with minimal impact
on your application code. Note, however, that Active Job does not currently have a mechanism for
detecting when a background job has failed.
