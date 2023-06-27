# Releasing Gems

## Semantic Versioning

> Version numbers and the way they change convey meaning about the underlying code and what has been modified from one version to the next.

In DLSS, we practice [semantic versioning](http://semver.org) for our gems.

## Bundler tasks

We use Bundler's rake tasks to release and push our gems. As long as the gem's Rakefile includes the following,
we have `build`, `install` and `release` Rake tasks available.

```
require 'bundler/gem_tasks'
```

These tasks make it easy to build gem releases, consistently tag the gem release in git, and push the gem to rubygems.org.

## Team ownership of gems

### Adding the DLSS service account to new gems

In order for all of us to maintain and release our gems, we expect all new gems managed by DLSS to have the DLSS service account added as a gem owner:

```console
$ gem owner base_indexer -a sul-devops-team@lists.stanford.edu
```

The service account (`sul-devops-team@lists.stanford.edu`) will ensure we all have access to our collective work.

### Adding/removing owners

As of late 2018, we use a Jenkins-based [solution](https://github.com/sul-dlss/rubygem-update-scripts) to ensure all Infrastructure and Access team members can release gems to rubygems.org for all gems in the DLSS portfolio. To add a new owner and remove an existing owner, create a GitHub pull request to modify [the list of Rubygems.org email addresses](https://github.com/sul-dlss/rubygem-update-scripts/blob/master/rubygems-emails.txt).

See the [README](https://github.com/sul-dlss/rubygem-update-scripts/blob/master/README.md) for more information about how this works.

## Steps to release a new version of a gem

1. Update the version of the gem in the default branch (typically `main`) according to the above [semantic versioning](http://semver.org) guidelines.
1. Run `rake release` in the gem directory
1. Add appropriate [release notes and publish the release](https://help.github.com/articles/creating-releases/)
