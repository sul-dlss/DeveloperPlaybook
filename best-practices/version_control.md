# Version Control

DLSS projects should be developed using a version control system, and, by default, using git. Current practice is to host our git repositories on [github](https://github.com).

DLSS projects, by default, should be public repositories within the [sul-dlss](https://github.com/sul-dlss) organization. In the case of some multi-institutional projects or collaborative development, these projects may live in other organizations.

By centralizing project hosting, it is easier for our colleagues to find and collaborate on our projects.

Projects should be public in order to share broadly with our colleagues at other institutions, who may learn from our approach (if not be able to run the code themselves) or offer useful critiques. Public repositories also encourage us to develop reusable code that follows best-practices around configuration and deployment.

If a project must be private, the `README.md` should note why (and, ideally, follow the principle of "A reason is good when you can convince a teammate."). Good reasons to keep a project private include:

- "Security concerns" about making the code public (not security by obscurity, but e.g. code migrated from legacy systems that may contain bad practices in the commit history)

## Committing Ethos

- Commit early, commit often. This is important both to ensure your work is safe, but also to get early feedback from colleagues.
- Don't break main (and the corollary, don't push directly to main; use branches and pull requests in order to give continuous integration, etc a chance to run)
- Don't rewrite public history
- Keep main releaseable. If you notice that it is broken (say, because of dependencies breaking), fix it.


## Commit Messages

Commits should have useful commit messages. A commit message is useful if it addresses these questions:

> Why is this change necessary?
>
> This question tells reviewers of your pull request what to expect in the commit, allowing them to more easily identify and point out unrelated changes.
>
> How does it address the issue?
>
> Describe, at a high level, what was done to affect change. Introduce a red/black tree to increase search speed or Remove <troublesome gem X>, which was causing <specific description of issue introduced by gem> are good examples.
>
> If your change is obvious, you may be able to omit addressing this question.
>
> What side effects does this change have?
>
> This is the most important question to answer, as it can point out problems where you are making too many changes in one commit or branch. One or two bullet points for related changes may be okay, but five or six are likely indicators of a commit that is doing too many things.
>
> Your team should have guidelines and rules-of-thumb for how much can be done in a single commit/branch.
> https://robots.thoughtbot.com/5-useful-tips-for-a-better-commit-message

See also:
 - http://chris.beams.io/posts/git-commit/
 - https://robots.thoughtbot.com/5-useful-tips-for-a-better-commit-message
 - http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html
