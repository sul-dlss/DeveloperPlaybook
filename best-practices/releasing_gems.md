# Releasing Gems

## Semantic Versioning

> Version numbers and the way they change convey meaning about the underlying code and what has been modified from one version to the next.

In DLSS, we practice [semantic versioning](http://semver.org) for our gems.

## Bundler tasks

We use Bundler's rake tasks to release and push our gems. By including:

```ruby
Bundler::GemHelper.install_tasks
```

In our `Rakefile`, bundler adds the `build`, `install` and `release` Rake tasks. These tasks make it easy to build gem releases, consistently tag the gem release in git, and push the gem to rubygems.org.

## Team ownership of gems

### Adding the DLSS service account as a gem owner

**NOTE**: Not sure if this is a relevant practice anymore. Leaving here for discussion.

In order for all of us to maintain and release our gems, we add a DLSS service account as a gem owner:

```console
$ gem owner base_indexer -a sul-devops-team@lists.stanford.edu
```

The service account will ensure we all have access to our collective work. 

### Run the gem bulk update script

As of late 2018, we use a [script](https://github.com/sul-dlss/infrastructure-update-scripts/blob/master/grant_revoke_gem_authority.rb) (adapted from a Samvera community script) to ensure all Infrastructure and Access team members can release gems to rubygems.org for all gems in the DLSS portfolio. Run this script after you push a gem for the first time to provide access to fellow DLSS developers. See the script for the prerequisite steps. If there are any errors in the output, discuss them in an appropriate channel (DLSS developers listserv, `#software-developers` Slack channel, etc.).

## Steps to release a new version of a gem

1. Update the version of the gem in the default branch (typically `master`) according to the above [semantic versioning](http://semver.org) guidelines.
1. Run `rake release` in the gem directory
1. Add appropriate [release notes and publish the release](https://help.github.com/articles/creating-releases/)
