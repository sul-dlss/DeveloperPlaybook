# Testing

## Why?

Testing helps show how the code was intended to be used, proves that it works for
the important cases, and prevents future developers from introducing regressions.
Having a good test suite enables us to refactor the code without worrying that
we're going to break something.

## When?

Every feature addition should be submitted with at least one (meaningful\*) test. Ideally,
all modified code should be tested (i.e., we aim for "diff coverage" of 100%). Even when
the intent isn't to modify behavior (e.g. a "pure refactoring" to pave the way for further
changes or to allow code re-use), it's possible to introduce regressions. In some cases,
existing code won't have tests, or the tests will be overly implementation specific. In those
cases we should prefer to pay down technical debt by writing better/any tests for such code,
but we also recognize time doesn't always permit this, or that time may not permit thorough
test writing in parallel with the code change (as in the case of an easily code reviewed fix
for an urgent production issue). Use your judgement, and leave an explanation on the PR if
test coverage could be better (maybe file an issue, too).

## How?

### Ruby

Ruby projects in DLSS should use the [RSpec](https://rspec.info/) framework for testing.
There is a `rspec-rails` gem for integrating with Rails projects. Much of the [Blacklight](http://projectblacklight.org/) and [Samvera](https://samvera.org/)
community have chosen to use RSpec; we should be consistent unless there is a reason not to. 

For best practices when using RSpec, see https://www.betterspecs.org/

### JavaScript

JavaScript should also be tested, but the multitude of javascript of testing libraries
can be daunting.  Take a look at https://medium.com/welldone-software/an-overview-of-javascript-testing-7ce7298b9870
for a concise guide.

Note that a couple of JS projects to which we contribute heavily (thus providing a snapshot of our current JS testing
approaches) are not in the [sul-dlss](https://github.com/sul-dlss/?q=&type=&language=javascript) org:
* https://github.com/ProjectMirador/mirador/
* https://github.com/LD4P/sinopia_editor/

\* "What is a meaningful test?" is currently out of scope for this markdown document 🙂
