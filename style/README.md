# Style

Stanford University's Digital Library Systems &amp; Services maintains various style enforcement practices for developers.

* [Ruby](/style/ruby)

## Introducing a Guide

We have a great many codebases in active use within the department, the majority of which do not currently conform to any style guide. We need a clear set of guidelines to help determine when it is appropriate to introduce a style guide.

A rule of thumb for when to introduce a style guide is as soon as you or someone on your team has cycles to do so. If you can do this at the outset of a project or work cycle, that is ideal. Otherwise, introduce a style guide the next time you touch a codebase that lacks one. Here are some suggestions to help you manage this:

* If your team already has a style guide they're comfortable using, use that one. If not, read the Baseline section immediately following this one.
* Run Rubocop to see how many violations there are to the baseline guide.
* Run Rubocop with the `--auto-correct` flag to automatically fix “easy” violations. (Bonus points if you run your test suite beforehand and afterwards and have a green builds both times.)
* Run Rubocop with the `--auto-gen-config` flag to generate the .rubocop_todo.yml file containing all of your current violations, then add `inherit_from: .rubocop_todo.yml` to the top of your .rubocop.yml file. This should make Rubocop run green.
* If you’re working on a project or a codebase that hasn’t previously considered how to handle style guides, consider looking at the TODOs as a team to come up with a style guide. Work with your Technical Lead on this.
* You might now add a ticket to your codebase where you can track progress on cleaning up TODOs.

## Baseline

If you’re starting up a new project or working on an existing project that doesn’t already have a more constrained style guide in place, you might consider starting with [DlssCops](https://github.com/sul-dlss/dlss_cops), a Rubocop configuration gem, as a baseline style guide. By baseline style guide, we mean merely a starting point or a default guide — **not** a set of inviolable best practices — that starts with the Rubocop defaults and then applies a small number of exclusions around styles that have been contentious in the past. As such, the DlssCops baseline is somewhat less strict than the Rubocop defaults. Rubocop groups its rules into categories, and DlssCops includes:

* All Lint cops enabled
* All Performance cops enabled
* All Metrics cops enabled
* All Rails cops enabled
* Most RSpec cops enabled
* Some Metrics cops configured to be less strict than the default values
* Two Style cops configured to be less strict than the default values
* One Style cop configured to prefer Rails style over Ruby style
* Twenty-One Style cops, about which there has been some disagreement, disabled

The exceptions above represent 33 deviations to the defaults provided by Rubocop and Rubocop-RSpec, which together have a total of 267 cops (as of the time this was drafted). That means the DlssCops baseline guide (as it is currently written) uses 88% of the default Ruby, Rails, and RSpec styles.

### Updating the Baseline

As our norms will naturally change over time — as will Rubocop's rules — so should our style guides. We need a lightweight process for determining how style guides change over time — this process should be inclusive and clear.

* Kick off the process by writing up an issue on the DlssCops GitHub repository. If you want to make sure certain colleagues are aware of the change you’re proposing, tag them in the issue.
* Discuss the issue on GitHub out in the open.
* If discussion is light, you might consider pinging your colleagues on Slack about the change.
* If there are no strong objections to the change, submit a pull request.
* Once merged, cut a new release of DlssCops, and projects and codebases that use the DlssCops baseline can point at the new release once they have cycles to deal with the changes it introduces.

## Project- or Codebase-Specific Styles

Project- or codebase-specific style guides can build upon the DlssCops baseline guide, or not use it at all, and make refinements or changes for teams that wish to work with an altered or more strict style guide. We recommend that every codebase pin all style guide-related dependencies down to the minor- or patch-level, including Rubocop, Rubocop-RSpec, and DlssCops if used, to avoid upstream changes to your codebase’s established style guide.

In general, we anticipate that a team's Technical Lead will be accountable for suggesting an initial style guide, and should have a process established — per the [work cycle kick-off checklist](https://consul.stanford.edu/pages/viewpage.action?pageId=151030108) — for conflict resolution. There are many possible ways to come up with a team style guide, including some of the ideas introduced by Sandi Metz during the Practical Object-Oriented Design course at Stanford in February 2016, such as allowing every team member to have a certain number of tokens that they can spend on configuring or making exceptions to the team style guide. Here's one example of how such a process could work:

1. The work cycle kicks off and DlssCops is chosen as a baseline guide.
1. The Technical Lead writes up a draft style guide based on the baseline, with additional restrictions (e.g., closer to the Rubocop defaults)
1. The Technical Lead gives all engineers on the team a certain number of tokens, which they may spend to tweak the proposed style guide.
1. Tokens are spent; changes are discussed/ratified; and the style guide is amended.

## Exceptions

[Declaring style exceptions](/style/exceptions.md)

## Selecting a Guide

With the possibility of defining multiple style guides — and the reality that our many codebases are in wildly different states with regard to how up-to-date they are, how well they are tested, whether they even work at all — here is a lightweight rubric for helping developers select a style guide appropriate for their codebase:

* If the codebase lives within a broader open-source community (e.g., Blacklight and Hydra), it is acceptable to use that community’s own style conventions as a starting point.
* If you're working on a project or a codebase that has previously used a specific style guide, consider holding a retrospective about its usefulness and using that guide again incorporating feedback from the retrospective.
* Else, work with your Technical Lead and the rest of your team on coming up with a project- or codebase-specific style guide. One starting point might be DlssCops.
