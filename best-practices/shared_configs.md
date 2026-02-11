# Shared Configs

Most of our Ruby-based projects use a convention we call "shared configs," which has three parts:

1. A long-lived git branch corresponding to a service or app in a given environment, which is managed in [a private GitHub repository](https://github.com/sul-dlss/shared_configs);
1. Configuration in [puppet](https://github.com/sul-dlss/puppet) associating an app in an environment with its shared configs branch name; and
1. Application code that automatically updates configs from the specified branch at deploy time, which is wired in via the [dlss-capistrano](https://github.com/sul-dlss/dlss-capistrano) gem (see: [deployment documentation](/deployment/capistrano.md))

For more information about how to set up shared_configs or how it all works, see [the shared_configs README](https://github.com/sul-dlss/shared_configs).

## Use of shared configs (Infra team practice)

Shared configs should be used for settings that vary by deployment environment. This excludes the following:

* All usernames / passwords / secrets: should be put in vault and set in puppet as env variables.
* Settings that must be env variables: should be set in puppet (e.g., TMPDIR).
* Honeybadger configuration: common Honeybadger configuration should be set in `honeybadger.yml` stored in the repo; environment specific Honeybadger settings should be set as env variables in puppet.
* Settings that don't vary by environment: common configuration should be set in `settings.yml` stored in the repo.

## Searching shared_configs branches

Because shared_configs is a large collection of unrelated git branches and the GitHub web user interface only allows searching against the HEAD branch of a repository, you might want to search your local checkout instead. For instance, when trying to answer questions about how many or which services are pointing at a particular API or URL. To do this, you might consider copy/pasting this script into an executable file on your PATH:

```bash
#!/usr/bin/env bash

# Find unique remote git branches (at HEAD) that match a search string

# Hack supporting mimicking associative arrays in bash v3
local branchmap="branchmap"

# Store commit IDs for searching
local commits=()

# Allow `for` to loop over lines instead of words
oldIFS=$IFS
IFS=$'\n'

# Get list of remote branches and their HEAD commits
for line in $(git ls-remote -q --heads --refs); do
    local commit=$(echo $line | cut -f1)
    local branch=$(echo $line | cut -f2)
    declare "branchmap_$commit=$branch"
    commits+=($commit)
done

# Only search commits that are HEADs for remote branches
for commit in $(git grep -li --no-color "$@" "${commits[@]}" | cut -d: -f1)
do
    # Print out matches using the map instead of e.g. `git branch -r --contains`. Why?
    #
    # Suppose branch1 has a commit abc123 that contains string "foobar".
    # Suppose branch2 is created off of branch1, and adds a commit def456
    # that removes the string "foobar". We search for "foobar" and commit
    # abc123 is returned. If we use `git branch -r --contains abc123`, both
    # branch1 and branch2 will be returned, but we don't want to see branch2
    # because its state at HEAD has "foobar" removed.
    local branchname="branchmap_$commit"
    echo ${!branchname}
done | sort | uniq | sed -e 's/refs\/heads/origin/'

# Clean-up: restore IFS and remove temporary variable
IFS=$oldIFS
```

Make sure to `git fetch origin` first to make sure you have the latest remote changes locally.

Then you can search all shared_configs branches, e.g.:

```shell
$ branchgrep aswsweb
origin/dor-services-app-prod
origin/sul-h2-prod
origin/sul-pub-prod
origin/sul-pub-prod-temp
```

And find all remote branches in the shared_configs GitHub repo that contain your search string.
