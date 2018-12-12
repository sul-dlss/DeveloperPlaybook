# Deprecating a Project

- Scrub the project for passwords, keys, and other sensitive information
- Is the project deployed to any machines? If so, make an [operations-tasks](https://github.com/sul-dlss/operations-tasks) ticket to decommission the machine.
- Is the project a dependency or service used by other sul-dlss projects?
- If the project is a gem, update the gem's description on RubyGems (e.g. DEPRECATED: this gem is no longer being supported)
- Move the repo to [sul-dlss-deprecated](https://github.com/sul-dlss-deprecated) ownership -- **NOTE** that you cannot move a private repo to the `sul-dlss-deprecated` org, in which case you can make the repo public first if appropriate or you can skip to the next step
- Consider [archiving the repository](https://help.github.com/articles/archiving-a-github-repository/)
