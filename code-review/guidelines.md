# Code Review

## Guidelines

### Reviewers and Authors Should

* work together on code reviews in a way that is timely, collegial, and respectful
* be respectful of others’ opinions
* feel free to write positive comments and ask constructive questions
* be open to learning
* be ready to compromise
* pursue team consensus as a means of resolving conflict
* consult an appropriate third party to arbitrate situations that result in deadlocks. The first person to be consulted should be the team’s designated technical lead for the current work cycle. Barring that, Mike Giarlo would be happy to serve in this role or to have issues escalated to him.

### Reviewers Should

* be clear and transparent about standards they use to review code
  * favor shared, documented standards
  * standards should be agreed upon by the department / team / existing code base
* evaluate the maintainability of the proposed code changes
  * look for patterns or assumptions in the code that may be brittle or otherwise detrimental to maintainability
  * evaluate the readability of the proposed code changes
    * according to shared, documented standards
    * according to personal judgment
  * evaluate test coverage of the proposed changes
    * suggest areas where coverage may be improved
  * evaluate changes in overall test coverage.  strive for full coverage of modified code when making changes (see [testing.md](../best-practices/testing.md)).
* be aware there are situations where the above guidelines may be bypassed
* think of requests for changes in terms of “must do” “should do” and “would be nice to do”
* collaborate with code authors to improve the code

### Authors Should

* **not** merge their own pull requests to common branches
  * unless explicitly allowed (e.g. working on a project alone and decision was made not to designate/solicit code reviewers)
* allow enough time for the code review process, and be prepared to escalate should code review not happen in a timely fashion
* indicate where code diverges from shared, documented standards or conventions
  * provide rationale for why, in the code if appropriate
  * be open to discussing and compromising on the trade-offs created
  * understand that differences of opinion are likely to arise in this situation
* know that suggestions for change are about the code, not the person

### Single-developer Projects

* **INCLUDES** contractor projects
* **SHOULD** use the code review process, including Pull Requests:
  * changes grouped for easy review by the coder
  * continuous integration build
  * better audit trail of grouped commits
  * way to link code changes with github issues
* **SHOULD** have one or more code reviewers appointed to your project
* **CAN** post to an appropriate Slack channel -- #software-developers or a project-specific channel -- or the dlss-dev mailing list asking for review of a specific PR.
