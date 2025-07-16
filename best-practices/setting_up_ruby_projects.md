# Setting up Ruby Projects

Many of the DLSS software projects are [Rails](http://rubyonrails.org/) applications and supporting Ruby gems. Consistency across projects can aid developers and reduce the overall maintenance burden on teams. This guide should be used as an outline on common expectations in DLSS Ruby and Rails projects. Not only should our projects reflect consistency internally, but should also reflect larger community expectations. These are not rules, but guidelines aimed to help the team.

## Open source by default
DLSS projects should be open source by default. See [licensing](/best-practices/licensing.md). Developers should _never_ check in api keys or private information (see `Configuration` section below). If you have any questions, check with fellow developers first.

## Deployment
We use [Capistrano](/deployment/capistrano.md) for deployment

## Configuration
Use the [ruby config](https://github.com/railsconfig/config) gem for configuration management and [shared_configs](/best-practices/shared_configs.md
) for environment-specific settings, but never use either of these for secrets (e.g., passwords, API keys). Instead, use [Vault](https://consul.stanford.edu/x/uYGqCQ) to store sensitive data.

## Continuous Integration
Projects should have implemented continuous integration so that proposed code changes are accompanied by test runs. The following services are used often:

 - [GitHub Actions](https://github.com/features/actions) or [CircleCI](https://circleci.com/) for running test suites
 - [CodeCov](https://app.codecov.io) or [CodeClimate](https://codeclimate.com/) for test coverage

### CodeCov Integration with GitHub
We currently have CodeCov configured with access to *selected* repos within the sul-dlss org, not all the repos. To grant access to new repos, add them via the [sul-dlss CodeCov app page on GitHub](https://github.com/organizations/sul-dlss/settings/installations/342140). *Note* that this URL is only accessible to GitHub sul-dlss org owners. You can ask the `#dlss-github-owners` Slack channel for help if you do not have access.

## Application Monitoring
See [Monitoring](/best-practices/monitoring.md).

## Database
Use the default `database.yml` from Rails unless there is a reason not to. Prefer to not tie your project to a specific SQL implementation unless necessary. You should use Rails migrations to specify and manage your database schema.

## Testing
Prefer [RSpec](https://rspec.info/) over other testing frameworks (e.g., `minitest`) for all Ruby projects. Rails projects should use the [RSpec Rails](https://github.com/rspec/rspec-rails) extensions for testing. See the [Testing](/best-practices/testing.md) chapter for more information.
