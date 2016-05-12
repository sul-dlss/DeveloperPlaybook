Code Review and Style Guides
============================

We want to write maintainable, bug-free code as a means to maximize our mandatory fun at work. It is more fun to work with code you consider maintainable and readable; having standards around the definitions of these qualities is a worthy endeavor, especially in a group as large as ours (software developers in DLSS). Code review is one tool for this.

Code reviews, at their best, help create readable, maintainable, well tested code. We hope to provide some guidelines to all reviewers that will favor productivity without detracting from code readability or maintainability. Similarly, there are expectations of code that is submitted for review.

Technologies that can facilitate these goals include: tests, code coverage, code dependency checkers (such as gemnasium and bundle-audit), and style guide tools such as rubocop.

Style-related choices can be a source of contention in the code review process; one goal of the Developer Playbook is to reduce conflict around style guides. Style guides have a place in professional software development organizations, helping to reduce the number of bugs and improve readability and maintainability across constantly changing projects and teams in an area where developers do not always agree; as we know, some developers care more deeply about code style than others do and we should expect that to be the case in DLSS where we have a large and diverse group of developers.

The point of the style guide is not to privilege one person’s preferences or values over another’s but rather to reflect consensus among DLSS developers — and that, as a living document, should change over time according to an open, inclusive process that reflects our norms as they evolve.

Absent a group style guide, style-related differences raised (potentially many times) in pull requests can cause delays that seem arbitrary and punitive, especially when they hold up merging long enough that a developer must rebase their work one or more times. This discussion about style is a good one to have — ideally before the pull request has been submitted. A style guide can help here, and allows developers involved with code review to focus on deeper concerns such as improvements to the algorithms and patterns employed.

Code review is a valuable practice for DLSS developers as we work together to improve our collective codecraft. That said, we should embrace that we are not our code and we should be open to constructive criticism about code that we write in terms of algorithms and patterns used and also in terms of others’ assessment of how readable, or confusing, the code we commit is.

With a group as large as ours, a style guide can play a significant role in focusing our code review process and helping us be happier developers. This is especially true factoring in how much of our work is embedded in open-source communities such as Hydra, where style guides are already commonly used — and which we should expect to be more involved with as we work towards greater alignment with open-source communities (e.g., Hydra) at DLSS.

The Developer Playbook puts in place a set of processes and practices — e.g., using code review via GitHub pull requests aided by style guides that promote readability and maintainability — that advances management of our many codebases in DLSS in a way that reflects our culture and our norms.

**N.B.** Developers need management to accept that arriving at and following shared practices such as style guides is going to increase costs (e.g., development time) at least at the outset, and managers should explicitly support developers taking the time necessary to follow these practices. Developers should be empowered to follow shared practices, and we should make sure to surface (e.g., in developer forums or X-Team Sprint Leads meetings) what we need from managers to succeed at following shared practices.
