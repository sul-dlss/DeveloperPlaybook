# Code Review

## Pull Request Practices in the Infrastructure Team

The Infrastructure Team uses the following practices with the goal of good communication around the status and needed actions for a PR.

* When reviewing a PR, change the PR assignee in GitHub to yourself. This allows others to be aware that the PR is receiving attention.  This does not mean that another developer cannot also review and comment on the PR at any point in its review, even after merging. Review by more than one developer is not wasted effort.
* Unassign yourself if you find you are no longer able to give the PR attention.
* Before merging a PR, make sure all PR assignees have submitted a review.
* Edit the PR title to have a prefix of [HOLD] if the PR requires further action before being merged. These may include:
  * The Product Owner needs to review and possibly test a deployment of the branch first.
  * Merging the PR is dependent on other actions. Indicate these in the comments in the PR.
* Remove the [HOLD] prefix before merging. This avoids the misperception that a PR that was supposed to be held was accidentally merged.
* Create a draft PR when the code is not ready for review. This is optional. Draft PRs provide the benefit of continuous integration testing and test coverage checks as well as comment and discussion during development. Change the draft PR to Ready for Review when you would like code review and the PR to be merged.
