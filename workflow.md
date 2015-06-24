# Workflow

## Table of Contents

1. [General](#general)
1. [GitHub Labels](#github-labels)

## General

  - We use the [Github Flow](http://scottchacon.com/2011/08/31/github-flow.html) if nothing else is specified:

  > - Anything in the master branch is deployable
  > - To work on something new, create a descriptively named branch off of master
  > - Commit to that branch locally and regularly push your work to the same named branch on the server
  > - When you need feedback or help, or you think the branch is ready for merging, open a pull request
  > - After someone else has reviewed and signed off on the feature, you can merge it into master
  > - Once it is merged and pushed to ‘master’, you can and should deploy immediately

## Github Labels

| Label              | Description                                                                     |
|--------------------|---------------------------------------------------------------------------------|
| pr:needs-review    | Pull request is ready to be reviewed.                                           |
| pr:needs-revision  | Pull request has been reviewed and needs further adjustments.                   |
| prio:critical      | Drop everything you're working on. This needs to be fixed immediately.          |
| prio:high          | This is the next thing to work on and should be finished in the next days.      |
| prio:low           | Nice to have.                                                                   |
| status:blocked     | Someone cannot continue to work on it (e.g. until some other work is finished). |
| status:in-progress | Someone is working on it.                                                       |
| type:bug           | Reported bug. It doesn't have to be confirmed as a bug by a team member yet.    |
| type:enhancement   | Making existing stuff better.                                                   |
| type:new-feature   | Confirmed feature which will be implmeneted in the near future.                 |
| type:question      | Discussing stuff.                                                               |
| type:refactoring   | Restructuring existing code without changing its external behavior.             |
| invalid            | This issue is a duplicate (close it with the comment "duplicate of #123") or won't be fixed. Always leave a comment. |



