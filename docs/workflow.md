# Workflow

## Table of Contents

1. [General](#general)
1. [Before you begin](#before-you-begin)
1. [Working on a feature or bugfix](#working-on-a-feature-or-bugfix)
1. [Using pull requests](#using-pull-requests)
1. [Submitting code for review](#submitting-code-for-review)
1. [Code review](#code-review)
1. [Merge and deploy](#merge-and-deploy)
1. [GitHub Labels](#github-labels)
1. [Further Reading](#further-reading)

## General

  - We use the [Github Flow](http://scottchacon.com/2011/08/31/github-flow.html) if nothing else is specified:

  > - Anything in the master branch is deployable
  > - To work on something new, create a descriptively named branch off of master
  > - Commit to that branch locally and regularly push your work to the same named branch on the server
  > - When you need feedback or help, or you think the branch is ready for merging, open a pull request
  > - After someone else has reviewed and signed off on the feature, you can merge it into master
  > - Once it is merged and pushed to ‘master’, you can and should deploy immediately

## Before you begin

  - Install [Git](https://git-scm.com/) and learn the [basics](https://try.github.io/)
  - If you don't already have one, create a [GitHub](http://www.github.com) account
  - Clone the project onto your computer: `git clone git@github.com:example/project.git`

## Working on a feature or bugfix

  - Whenever you're working on a new feature or a fix, you should create a new branch. It's important that it's created off the `master` branch. Follow this branch naming convention: `i<issue number>-<short description>`:

    ```sh
    git checkout -b i47-auto-save
    ```

  - [Commit early and often](http://www.databasically.com/2011/03/14/git-commit-early-commit-often/).

  - Write good commit messages:
    - Reference the GitHub issue with `#<issue number>`
    - Use the imperative mode: "add", "change", "fix" instead of "added", "changed", "fixed".
    - Capitalize the subject line.
    - Do not end the subject line with a period - it's a title and titles don't end with a period.
    - Use [this template](http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html) for longer commit messages:
      - Use the body to explain *why* you made these changes.
      - Separate subject from body with a blank line.

  ```sh
  git commit -m "Add GZIP compression #55"
  ```

  - Periodically, changes made to `master` (if any) should be merged back into your branch.

    ```sh
    git merge master
    ```

## Using pull requests

  - It doesn't matter if you open a pull request when you start a feature or wait until the feature is implemented.

## Submitting code for review

  -  Before submitting your code, make sure you fulfill the following requirements:
    - Your branch is up to date with the `master` branch.
    - Your changes adhere to our [coding standards](https://github.com/infektweb/conventions).
    - All existing tests pass.
    - You've added tests for your new code.
    - Your code is documented.

  - Label your pull request with `pr:needs-review`.

## Code review

  - Everybody in the team can review a pull request which has the label `pr:needs-review`.

  - Checklist:
    - Is the branch mergeable?
    - Does the code work?
    - Does the code make sense?
    - Is the code well-structured?
    - Does the code conform to our [coding standards](https://github.com/infektweb/conventions)?
    - Are variables properly defined with consistent and meaningful names?
    - Is there any redundant or duplicate code?
    - Are available libraries being used effectively?
    - Is there any commented out code?
    - Are edge cases handled properly?
    - Are data inputs checked and output values encoded?
    - Is the code adequately documented ("why", complex and critical sections)?
    - Do tests exist and are they comprehensive?
    - Is the performance acceptable?

  - Add a comment and set the label `pr:needs-revision`, if you found something that can be improved.

  - Leave a `+1` or `:+1:` comment, if it passed your review and doesn't need modifications.


## Merge and deploy

  - Once at least 1 and preferably 2 people have reviewed the code, the pull request can be merged into the `master` branch.

  ![Merge Pull Request](https://github.com/infektweb/conventions/blob/master/docs/images/merge-pull-request.png)

  - **Never merge your own code.**

  - As soon as you merge the pull request into the `master` the tests are kicked off on our CI server and the deployment starts, if the tests pass.

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


## Further Reading

  - [Pro Git book](http://git-scm.com/book)
  - [Understanding the GitHub Flow](https://guides.github.com/introduction/flow/)
