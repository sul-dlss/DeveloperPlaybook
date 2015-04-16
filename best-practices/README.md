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

Ruby Gems
---------

* Declare dependencies in the `<PROJECT_NAME>.gemspec` file.
* Reference the `gemspec` in the `Gemfile`.
* Use [Bundler] to manage the gem's dependencies.
* Use [Travis CI] for Continuous Integration, indicators showing whether GitHub
  pull requests can be merged, and to test against multiple Ruby versions.

[Bundler]: http://bundler.io
[Travis CI]: http://travis-ci.org

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

Browsers
--------

Support commonly used browsers first, and use [polyfills] to provide reasonable behavior for older browsers.

[polyfills]: https://github.com/Modernizr/Modernizr/wiki/HTML5-Cross-Browser-Polyfills
