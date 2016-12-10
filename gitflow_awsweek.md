# aws/aws-week-in-review Git Workflow

*Rev 0.2*

## Contribution Process

There is a 3 step process for contributions when using git and an editor like Vim or Atom:

* Commit changes to a git branch.
* Create a GitHub Pull Request for your change,
following the instructions in the pull request template.
* For any significant changes, perform a "Code Review"
with the project maintainers on the pull request.

## Use git

`aws/aws-week-in-review` is maintained on GitHub.
To contribute to `aws/aws-week-in-review`,
such as submitting a pull request, requires using GitHub and git.
The sections below describe how to use git
to set up the `aws/aws-week-in-review` repository,
keep it current and synchronized,
and how to use branches to submit pull requests.

## Set Up Repo
Use the following steps to set up a development
repository for `aws/aws-week-in-review`:

* Set up a GitHub account.
* Fork the https://github.com/aws/aws-week-in-review repository to your GitHub account.
* Clone your fork of the https://github.com/aws/aws-week-in-review repository:

`$ git clone https://github.com/YourGitHubUserName/aws-week-in-review.git`

* From the command line, browse to the `aws-week-in-review/` directory:

`$ cd aws-week-in-review/`

* From the `aws-week-in-review/` directory, add a remote
named `aws-week-in-review` pointing to the base `aws/aws-week-in-review` directory:

`$ git remote add aws-week-in-review https://github.com/aws/aws-week-in-review.git`

* Verify:

`$ git remote -v`

which should return something like,
showing the base Github repositiory and your own repo:

```
aws-week-fork  https://github.com/aws/aws-week-in-review.git (fetch)
aws-week-fork  https://github.com/aws/aws-week-in-review.git (push)
origin  https://github.com/YourGitHubUserName/aws-week-in-review.git (fetch)
origin  https://github.com/YourGitHubUserName/aws-week-in-review.git (push)
```

or

```
aws-week-fork	git@github.com:rjhintz/aws-week-in-review.git (fetch)
aws-week-fork	git@github.com:rjhintz/aws-week-in-review.git (push)
origin	git@github.com:aws/aws-week-in-review.git (fetch)
origin	git@github.com:aws/aws-week-in-review.git (push)
```

## Keep Local Master Current With Fork
Use the following steps to keep the your local master branch up to date with your Github fork.

* Run:

`$ git checkout master`

* And then run:

`$ git pull --rebase`

## Sync Local Master with Base Project Master
Use the following steps to synchronize locally with the base `aws/aws-week-in-review` master branch.

* Run:

`$ git fetch aws-week-fork`

* And then run:

`$ git rebase aws-week-fork/master master`

## Sync Your Github Fork with Both Local and Base
If your Github fork is behind the base master, update the fork to the level of your local, which you just rebased  with the base repository:

`git push`

You can check your Github fork to see if it is now even with the base repository.
If so, it will say:

`This branch is even with aws:master.`

#### Note on Rebase
Use `rebase` instead of `merge` to ensure that a linear history is
maintained that does not include unnecessary merge commits.
`rebase` will also rewind, apply,
and then reapply commits to the `master` branch.

## Use a Topic Branch
Commits to the `aws/aws-week-in-review` repositories should never be made against the master branch.
Use a topic branch instead.

A topic branch solves a single and unique problem and often maps
closely to an issue being tracked in the repository.
For example, a topic branch could be used to add significant content associated with an issue.

Ideally, a topic branch is named in a way that associates it
closely with the issue it is attempting to resolve.
This helps ensure that others may easily find it.

Use the following steps to create a topic branch:

* For a brand new clone of the `aws/aws-week-in-review` repository
that was created using the steps listed earlier, fetch the aws-week-in-review remote:

`$ git fetch aws-week-in-review`

* Create an appropriately named tracking branch:

`$ git checkout --track -b aws-week-<topic-name> aws-week-in-review/master`

Sets up a topic branch to track the base aws aws-week-in-review/master.
This allows commits to be easily rebased prior to merging.

* Make your changes, review, and then commit them.

* Review

`$ git diff`

* First review what files are staged for commit, if any

`$ git status`

* Add changed file or possibly files to be staged

`$ git add <filename>`

* Commit tracked files:

`$ git commit -am "commit message"`

* Rebase the commits against the base `aws-week-in-review/master`.
After work in the topic branch is finished,
rebase these commits against the upstream master.
Do this manually with `git fetch` followed by a `git rebase`
or use `git pull --rebase`.

git will let you know if there are any problems.
In the event of problems, fix them as directed,
and then mark as fixed with a `git add`,
and then continue the rebase process using `git rebase --continue`.

For example, first fetch from the base repository:

`$ git fetch aws-week-in-review`

followed by:

`$ git rebase aws-week-in-review/master aws-week-<topic-name>`

Or:

`$ git pull --rebase`

* Push the local topic branch to GitHub:

`$ git push origin aws-week-<topic-name>`

* Send a GitHub pull request for the changes,
including `#<issue>`, if applicable.

You can [include keywords in your commit
messages](https://help.github.com/articles/closing-issues-via-commit-messages/)
to automatically close issues in GitHub, though this may not be appropriate for
many issues that have ongoing content additions or revisions.

* Navigate to the base project
* Select `New pull request` button
* Compare changes using `compare across forks` link
* Make sure base fork is base project and its base is `master`
* Change `head fork` to your fork and compare is `master`
' Redview pull request text and review change diff


## Delete Branch
After work has been merged by the branch maintainer,
the topic branch is no longer necessary and should be removed.

* Synchronize the local master:

`$ git checkout master`

followed by:

`$ git pull --rebase`

* Remove the local branch
 using `-d` to ensure that it has been merged by upstream.
 This option will not delete a branch that is
 not an ancestor of the current `HEAD`. From the git man page:

```
-d
  Delete a branch. The branch must be fully merged in HEAD.
-D
  Delete a branch irrespective of its merged status.
  ```

* Remove the local branch:

`$ git branch -d aws-week-in-<topic-name>`

Or remove the remote branch by using the full syntax to push`
and by omitting a source branch:

`$ git push origin :aws-week-<topic-name>`
