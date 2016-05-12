# Code Review

## Goals

On the face of it, the point of code review is:

* to improve code accepted into common branches of our projects,
* to improve our collective programming skills, and
* to share knowledge about project code.

Ultimately, our goal is to build a culture of excellence: producing high quality, maintainable code.

### Code-oriented Goals

* Maintainability
  * including audit trail (appropriately atomic commits with informative descriptions)
* DRY - Don’t Repeat Yourself.  “Has this already been done?”  Avoid copypasta.
  * BUT:  meta-program with great care
* Pertinence - “Is this the *right place* to do it?”
* Correctness - “Does it do what we wanted?”
* Verifiability - i.e., testing.  “How do we know it does it?  Will we know if it breaks later?”
* QA - “Does it do it how we wanted? Are we going to have to redo it again given X? Can we do this better?”
* Performance - “Is it going to bog down with real quantities of real data?”
* Robustness - Logging, exception handling, safe failure modes.
* Usability - “Can consumers understand how to use this? Is it documented? readable?”
* Reusability - discrete functionality needed in other projects shouldn’t be buried in monolithic apps.  Build modularity for future DRYness.
* Deployability - Your laptop is not our production environment.  Make OPS happy.
* Security - Experienced developers will more readily recognize vulnerable patterns.

### Organization-oriented Goals

* Build a culture of excellence: skilled developers that function as a team.
* Promote open culture where questions about code are welcome.
* Share knowledge of “better” or “more idiomatic” or “more performant” ways
* Reduce [Bus Factor](https://en.wikipedia.org/wiki/Bus_factor) (aka Truck Factor): code should be understood by multiple people and responsibility should be shared.
* Promote familiarity with different parts of the department / community codebase.
* Promote familiarity with different languages, components and features being used.
* Contextual awareness of where your work bumps against, compromises or facilitates others’ work.
* Bigger picture: understand the “why” of various features, classes, code, etc.
