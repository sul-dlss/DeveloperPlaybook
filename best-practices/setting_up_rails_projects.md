# Setting up Rails Projects

Many of the DLSS software projects are [Ruby on Rails](http://rubyonrails.org/) applications. Consistency across projects can aid developers and reduce the overall maintenance burden on teams. This guide should be used as an outline on common expectations in DLSS Ruby on Rails projects. Not only should our projects reflect consistency internally, but should also reflect larger community expectations. These are not rules, but guidelines aimed to help the team.

## Open source by default
DLSS projects should be open source by default. See [licensing](/best-practices/licensing.md). Developers should _never_ check in api keys or private information. Use the [config](https://github.com/railsconfig/config) gem instead. If you have any questions, check with dev-ops first.

## Configuration
Use the [config](https://github.com/railsconfig/config) gem for configuration management.

## Continuous Integration
Projects should have implemented continuous integration. The following services are used often:

 - [travis-ci](https://travis-ci.org/) for running test suites
 - [hound-ci](https://houndci.com/repos) for style checking
 - [coveralls](https://coveralls.io/) for code coverage

## Application Monitoring
See [Monitoring](/best-practices/monitoring.md).

## Database
Use the default `database.yml` from Rails unless there is a reason not to. Prefer to not tie your project to a specific SQL implementation unless necessary. You should use Rails migrations to specify and manage your database schema.

## Testing
Rails projects in DLSS should use the [RSpec Rails](https://github.com/rspec/rspec-rails) framework for Ruby testing. Much of the [Blacklight](http://projectblacklight.org/) and [Project Hydra](https://projecthydra.org/) community have chosen to use RSpec, we should be consistent unless there is a reason not to.

JavaScript should also be tested. Rails projects have had success in using [teaspoon](https://github.com/modeset/teaspoon) for this.
