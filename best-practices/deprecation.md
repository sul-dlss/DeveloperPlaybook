# Deprecating a Project

- Scrub the project (and its git history) for passwords, keys, and other sensitive information
  - There are several ways to do this and if you don't already have tools/techniques you lean on here, you can consider using [truffleHog](https://github.com/dxa4481/truffleHog) for finding secrets and [BFG Repo-Cleaner](https://rtyley.github.io/bfg-repo-cleaner/) for wiping them out. Note too that you will need to force-push the changes to the repository you're working with.
- Is the project deployed to any machines? If so, make an [operations-tasks](https://github.com/sul-dlss/operations-tasks) ticket to decommission the machine.
- Is the project a dependency or service used by other sul-dlss projects?
- If the project is a gem, update the gem's description on RubyGems (e.g. DEPRECATED: this gem is no longer being supported)
- Move the repo to [sul-dlss-deprecated](https://github.com/sul-dlss-deprecated) ownership 
- [OPTIONAL] Change the README and/or GitHub repository description to provide context around the deprecation (why? when? anything superseding? etc.)
- [OPTIONAL] [Archive the repository](https://help.github.com/articles/archiving-a-github-repository/) on GitHub, making it read-only
