Best Practices
==============

Adapted from https://github.com/thoughtbot/guides/blob/master/best-practices/README.md

A guide for programming well.

General
-------

* These are not to be blindly followed; strive to understand these and ask
  when in doubt.
* Don't duplicate the functionality of a built-in library.
* Don't swallow exceptions or "fail silently."
* Don't write code that guesses at future functionality.
* Exceptions should be exceptional.
* [Keep the code simple].

[Keep the code simple]: http://www.readability.com/~/ko2aqda2

Licensing
---------

As a matter of practice, DLSS publishes software into publicly accessible code repositories. This facilitates the review, exchange, reuse and possible code contributions from other sites--a key part of our development strategy and methodology. As best practice, we endeavor to put a clear license on this code so others know what they may and may not do with it. DLSS staff should release it under an open source license.

* When contributing to an existing codebase that has an approved OSS license, we should contribute the code back under the this same license.
* When developing new code, then it should use an [Apache 2 license] as the default.
* The copyright holder for most work is "The Board of Trustees of the Leland Stanford Junior University"

[Apache 2 license]: /best-practices/examples/apache2-license

Object-Oriented Design
----------------------

* Avoid global variables.
* Avoid long parameter lists.
* Limit collaborators of an object (entities an object depends on).
* Limit an object's dependencies (entities that depend on an object).
* Prefer composition over inheritance.
* Prefer small methods. Between one and five lines is best.
* Prefer small classes with a single, well-defined responsibility. When a
  class exceeds 100 lines, it may be doing too many things.
* [Tell, don't ask].

[Tell, don't ask]: http://robots.thoughtbot.com/post/27572137956/tell-dont-ask

Version Control
---------------

* Use git
* Host your projects on github (it's surprising how often it's useful to share Stanford-specific code, even just for the general approach to a problem)
* Prefer using public repositories.
* Prefer hosting your project in the `sul-dlss` organization so it is easier to discover. If it makes sense to move it to a separate organization later, github makes the proces easy and seamless.
* Provide a short description for your github project (and a URL, if applicable)
* Prefer pull requests instead of pushing directly to master (and give continuous integration a chance to run before merging)
* Before merging a branch, consider squashing the commits to only salient, well-described commits (e.g. not "fix typo", "fix a bug in the last commit", "why won't you work!!?!!?" 
* Don't rewrite public history
* Write [good commit messages]. If you're fixing an issue, reference the issue in the commit message, but also include additional context.

[good commit messages]: https://robots.thoughtbot.com/5-useful-tips-for-a-better-commit-message

Deployment
----------
* Document how to deploy the project in (or linked to from) the readme
* Use a reproducable deployment strategy
* Have a branch that is always deployable
* Use commonly used deployment software (e.g. capistrano for ruby-based applications)

Ruby
----

* Avoid optional parameters. Does the method do too much?
* Avoid monkey-patching.
* Prefer classes to modules when designing functionality that is shared by
  multiple models.
* Prefer `private` when indicating scope. Use `protected` only with comparison
  methods like `def ==(other)`, `def <(other)`, and `def >(other)`.

Ruby Gems
---------

* Declare dependencies in the `<PROJECT_NAME>.gemspec` file.
* Reference the `gemspec` in the `Gemfile`.
* Use [Bundler] to manage the gem's dependencies.
* Use [Travis CI] for Continuous Integration, indicators showing whether GitHub
  pull requests can be merged, and to test against multiple Ruby versions.

[Bundler]: http://bundler.io
[Travis CI]: http://travis-ci.org

Rails
-----

* [Add foreign key constraints][fkey] in migrations.
* Avoid bypassing validations with methods like `save(validate: false)`,
  `update_attribute`, and `toggle`.
* Avoid instantiating more than one object in controllers.
* Avoid naming methods after database columns in the same class.
* Don't change a migration after it has been merged into master if the desired
  change can be solved with another migration.
* Don't reference a model class directly from a view.
* Don't return false from `ActiveModel` callbacks, but instead raise an
  exception.
* Don't use instance variables in partials. Pass local variables to partials
  from view templates.
* Don't use SQL or SQL fragments (`where('inviter_id IS NOT NULL')`) outside of
  models.
* If there are default values, set them in migrations.
* Keep `db/schema.rb` under version control.
* Use only one instance variable in each view.
* Use `_url` suffixes for named routes in mailer views and [redirects].  Use
  `_path` suffixes for named routes everywhere else.
* Validate the associated `belongs_to` object (`user`), not the database column
  (`user_id`).
* Use `db/seeds.rb` for data that is required in all environments.
* Prefer `cookies.signed` over `cookies` to [prevent tampering].
* Prefer `Time.current` over `Time.now`
* Prefer `Date.current` over `Date.today`
* Prefer `Time.zone.parse("2014-07-04 16:05:37")` over `Time.parse("2014-07-04 16:05:37")`
* Use `ENV.fetch` for environment variables instead of `ENV[]`so that unset
  environment variables are detected on deploy.

[fkey]: http://robots.thoughtbot.com/referential-integrity-with-foreign-keys
[redirects]: http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.30
[prevent tampering]: http://blog.bigbinary.com/2013/03/19/cookies-on-rails.html

Testing
-------

* Avoid `any_instance` in rspec-mocks and mocha. Prefer [dependency injection].
* Avoid using instance variables in tests.
* Disable real HTTP requests to external services with
  `WebMock.disable_net_connect!`.
* Don't test private methods.
* Test background jobs with a [`Delayed::Job` matcher].
* Use [stubs and spies] \(not mocks\) in isolated tests.
* Use a single level of abstraction within scenarios.
* Use an `it` example or test method for each execution path through the method.
* Use [assertions about state] for incoming messages.
* Use stubs and spies to assert you sent outgoing messages.
* Use integration tests to execute the entire app.
* Use non-[SUT] methods in expectations when possible.

[dependency injection]: http://en.wikipedia.org/wiki/Dependency_injection
[`Delayed::Job` matcher]: https://gist.github.com/3186463
[stubs and spies]: http://robots.thoughtbot.com/post/159805295/spy-vs-spy
[assertions about state]: https://speakerdeck.com/skmetz/magic-tricks-of-testing-railsconf?slide=51
[SUT]: http://xunitpatterns.com/SUT.html

Bundler
-------

* Specify the [Ruby version] to be used on the project in the `Gemfile`.
* Use a [pessimistic version] in the `Gemfile` for gems that follow semantic
  versioning, such as `rspec`, `factory_girl`, and `capybara`.
* Use a [versionless] `Gemfile` declarations for gems that are safe to update
  often, such as pg, thin, and debugger.
* Use an [exact version] in the `Gemfile` for fragile gems, such as Rails.

[Ruby version]: http://bundler.io/v1.3/gemfile_ruby.html
[exact version]: http://robots.thoughtbot.com/post/35717411108/a-healthy-bundle
[pessimistic version]: http://robots.thoughtbot.com/post/35717411108/a-healthy-bundle
[versionless]: http://robots.thoughtbot.com/post/35717411108/a-healthy-bundle

Background Jobs
---------------

* Store IDs, not `ActiveRecord` objects for cleaner serialization, then re-find
  the `ActiveRecord` object in the `perform` method.

Email
-----

* Use a tool like [ActionMailer Preview] to look at each created or updated mailer view
  before merging. Use [MailView] gem unless using Rails version 4.1.0 or later.

[Amazon SES]: http://robots.thoughtbot.com/post/3105121049/delivering-email-with-amazon-ses-in-a-rails-3-app
[SendGrid]: https://devcenter.heroku.com/articles/sendgrid
[MailView]: https://github.com/37signals/mail_view
[ActionMailer Preview]: http://api.rubyonrails.org/v4.1.0/classes/ActionMailer/Base.html#class-ActionMailer::Base-label-Previewing+emails

CSS
---

* Use Sass.

Sass
----

* Prefer `overflow: auto` to `overflow: scroll`, because `scroll` will always
  display scrollbars outside of OS X, even when content fits in the container.
* Use `image-url` and `font-url`, not `url`, so the asset pipeline will re-write
  the correct paths to assets.
* Prefer mixins to `@extend`.

Browsers
--------

Support commonly used browsers first, and use [polyfills] to provide reasonable behavior for older browsers.

[polyfills]: https://github.com/Modernizr/Modernizr/wiki/HTML5-Cross-Browser-Polyfills
