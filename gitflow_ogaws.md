# OG-AWS Git Workflow

## Contribution Process

There is a 3 step process for contributions:

* Commit changes to a git branch.
* Create a GitHub Pull Request for your change,
following the instructions in the pull request template.
* For any significant changes, perform a "Code Review"
with the project maintainers on the pull request.

## Use git

OG-AWS is maintained on GitHub. To contribute to OG-AWS,
such as submitting a pull request, requires using GitHub and git.
The sections below describe how to use git to set up the OG-AWS repository,
keep it current and synchronized,
and how to use branches to submit pull requests.

## Set Up Repo
Use the following steps to set up a development repository for OG-AWS:

* Set up a GitHub account.
* Fork the https://github.com/open-guides/og-aws repository to your GitHub account.
* Clone your fork of the https://github.com/open-guides/og-aws repository:

`$ git clone https://github.com/YourGitHubUserName/og-aws.git`

* From the command line, browse to the `open-guides/` directory:

`$ cd og-aws/`

* From the `og-aws/` directory, add a remote named `og-aws` pointing to the base `open-guides/og-aws` directory:

`$ git remote add og-aws https://github.com/open-guides/og-aws.git`

* Verify:

`$ git remote -v`

which should return something like:

```
og-aws  https://github.com/open-guides/og-aws.git (fetch)
og-aws  https://github.com/open-guides/og-aws.git (push)
origin  https://github.com/YourGitHubUserName/og-aws.git (fetch)
origin  https://github.com/YourGitHubUserName/og-aws.git (push)
```

## Keep Master Current
Use the following steps to keep the your local master branch up to date with your Github fork.

* Run:

`$ git checkout master`

* And then run:

`$ git pull --rebase`

## Sync Master
Use the following steps to synchronize with the base `open-guides/og-aws` master branch.

* Run:

`$ git fetch og-aws`

* And then run:

`$ git rebase og-aws/master master`

#### Note
Use `rebase` instead of `merge` to ensure that a linear history is
maintained that does not include unnecessary merge commits.
`rebase` will also rewind, apply,
and then reapply commits to the `master` branch.

## Use Branch
Commits to the OG-AWS repositories should never be made against the master branch.
Use a topic branch instead.

A topic branch solves a single and unique problem and often maps
closely to an issue being tracked in the repository.
For example, a topic branch to add significant content associated with an issue.

Ideally, a topic branch is named in a way that associates it
closely with the issue it is attempting to resolve.
This helps ensure that others may easily find it.

Use the following steps to create a topic branch:

* For a brand new clone of the OG-AWS repository
(that was created using the steps listed earlier), fetch the og-aws remote:

`$ git fetch og-aws`

* Create an appropriately named tracking branch:

`$ git checkout --track -b og-aws-<topic-name> og-aws/master`

Sets up a topic branch to track the base open-guides og-aws/master.
This allows commits to be easily rebased prior to merging.

* Make your changes, and then commit them:

`$ git status`

* And then run the following, which stages and commits tracked files:

`$ git commit -am "commit message`

* Rebase the commits against the base `og-aws/master`.
After work in the topic branch is finished,
rebase these commits against the upstream master.
Do this manually with `git fetch` followed by a `git rebase`
or use `git pull --rebase`.

git will let you know if there are any problems.
In the event of problems, fix them as directed,
and then mark as fixed with a `git add`,
and then continue the rebase process using `git rebase --continue`.

For example:

`$ git fetch og-aws`

followed by:

`$ git rebase og-aws/master og-aws-<topic-name>`

Or:

`$ git pull --rebase`

* Push the local topic branch to GitHub:

`$ git push origin og-aws-<topic-name>`

* Send a GitHub pull request for the changes,
including `#<issue>`, if applicable.

You can [include keywords in your commit
messages](https://help.github.com/articles/closing-issues-via-commit-messages/)
to automatically close issues in GitHub, though this may not be appropriate for
many issues that have ongoing content additions or revisions.

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

`$ git branch -d og-aws-<topic-name>`

Or remove the remote branch by using the full syntax to push`
and by omitting a source branch:

`$ git push origin :og-aws-<topic-name>`
