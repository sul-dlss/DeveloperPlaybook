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

## Adding the DLSS service account as a gem owner

In order for all of us to maintain and release our gems, we add a DLSS service account as a gem owner:

```console
$ gem owner base_indexer -a sul-devops-team@lists.stanford.edu
```

The service account will ensure we all have access to our collective work. 
