# Workflow

## Table of Contents

1. [General](#general)
1. [Before you begin](#before-you-begin)
1. [Working on a feature or bugfix](#working-on-a-feature-or-bugfix)
1. [Submitting code for review](#submitting-code-for-review)
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
  - Clone the project onto your computer:
  ```sh
  git clone git@github.com:example/project.git
  ```

## Working on a feature or bugfix

  - Whenever you're working on a new feature or a fix, you should create a new branch. It's important that it's created off the `master` branch. Follow this branch naming convention: `i<issue number>-<short description>`:
  
    ```sh
    git checkout -b i47-auto-save
    ```
  
  - [Commit early and often](http://www.databasically.com/2011/03/14/git-commit-early-commit-often/).

  - Write good commit messages:
  
    - Use the imperative mode: "add", "change", "fix" instead of "added", "changed", "fixed".
    - Capitalize the subject line.
    - Do not end the subject line with a period - it's a title and titles don't end with a period.
    - Use the body to explain *why* you made these changes.
    - Separate subject from body with a blank line.
    - Here is a [template originally written by Tim Pope](http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html):
  
    ```
    Short (50 chars or less) summary of changes

    More detailed explanatory text, if necessary.  Wrap it to
    about 72 characters or so.  In some contexts, the first
    line is treated as the subject of an email and the rest of
    the text as the body.  The blank line separating the
    summary from the body is critical (unless you omit the body
    entirely); tools like rebase can get confused if you run
    the two together.
    
    Further paragraphs come after blank lines.
    
      - Bullet points are okay, too
    
      - Typically a hyphen or asterisk is used for the bullet,
        preceded by a single space, with blank lines in
        between, but conventions vary here
    ```
   

  - Periodically, changes made to `master` (if any) should be merged back into your branch.
  
    ```sh
    git merge master
    ```

## Submitting code for review

  -  Before submitting your code, make sure you fulfill the following requirements:
    - Your branch is up to date with the `master` branch.
    - Your changes adhere to our [coding standards](https://github.com/infektweb/conventions).
    - All existing tests pass.
    - You've added tests for your new code.
    - Your code is documented.

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
