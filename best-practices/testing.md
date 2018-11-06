# Testing

Testing helps show how the code was intended to be used, proves that it works for
the important cases, and prevents future developers from introducing regressions.
Having a good test suite enables us to refactor the code without worrying that
we're going to break something.  Every feature addition should be submitted with
at least one test.

Ruby projects in DLSS should use the [RSpec](https://rspec.info/) framework for testing.
There is a `rspec-rails` gem for integrating with Rails projects. Much of the [Blacklight](http://projectblacklight.org/) and [Samvera](https://samvera.org/)
community have chosen to use RSpec; we should be consistent unless there is a reason not to.

JavaScript should also be tested, but the multitude of javascript of testing libraries
can be daunting.  Take a look at https://medium.com/welldone-software/an-overview-of-javascript-testing-in-2018-f68950900bc3
for a concise guide.
