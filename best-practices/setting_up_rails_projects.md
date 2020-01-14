# Setting up Rails Projects

Many of the DLSS software projects are [Ruby on Rails](http://rubyonrails.org/) applications. Consistency across projects can aid developers and reduce the overall maintenance burden on teams. This guide should be used as an outline on common expectations in DLSS Ruby on Rails projects. Not only should our projects reflect consistency internally, but should also reflect larger community expectations. These are not rules, but guidelines aimed to help the team.

## Open source by default
DLSS projects should be open source by default. See [licensing](/best-practices/licensing.md). Developers should _never_ check in api keys or private information. Use the [config](https://github.com/railsconfig/config) gem instead. If you have any questions, check with dev-ops first.

## Deployment
We use capistrano for deployment. [More information](deployment.md)

## Configuration
Use the [config](https://github.com/railsconfig/config) gem for configuration management. Use [shared-configs](https://github.com/sul-dlss/shared_configs) to store senstive data.

## Continuous Integration
Projects should have implemented continuous integration. The following services are used often:

 - [travis-ci](https://travis-ci.com/) or [CircleCI](https://circleci.com/) for running test suites
 - [codeclimate](https://codeclimate.com/) for code coverage and code quality

## Application Monitoring
See [Monitoring](/best-practices/monitoring.md).

## Database
Use the default `database.yml` from Rails unless there is a reason not to. Prefer to not tie your project to a specific SQL implementation unless necessary. You should use Rails migrations to specify and manage your database schema.

## Testing
Rails projects in DLSS should use the [RSpec Rails](https://github.com/rspec/rspec-rails) framework for Ruby testing. See the [Testing](/best-practices/testing.md) chapter for more information.
