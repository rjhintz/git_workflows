## Contribution Process

We have a 3 step process for contributions:

* Commit changes to a git branch,
making sure to sign-off those changes for the Developer Certificate of Origin.
* Create a GitHub Pull Request for your change,
following the instructions in the pull request template.
* Perform a Code Review with the project maintainers on the pull request.

## Use git

Chef is maintained on GitHub. To contribute to Chef,
such as submitting a pull request, requires using GitHub and git.
The sections below describe how to use git to set up the Chef repository,
keep it current and synchronized,
and how to use branches to submit pull requests.

## Set Up Repo
Use the following steps to set up a development repository for Chef:

* Set up a GitHub account.
* Fork the https://github.com/chef/chef repository to your GitHub account.
* Clone the https://github.com/chef/chef repository:

`$ git clone git@github.com:yourgithubusername/chef.git`

* From the command line, browse to the `chef/` directory:

`$ cd chef/`

* From the chef/ directory, add a remote named chef:

`$ git remote add chef git://github.com/chef/chef.git`

* Verify:

`$ git config --get-regexp "^remote\.chef"`

which should return something like:

`remote.chef.url git://github.com/chef/chef.git`

`remote.chef.fetch +refs/heads/*:refs/remotes/chef/*`

* Adjust your branch to track the chef/master remote branch:

`$ git config --get-regexp "^branch\.master"`

which should return something like:

`branch.master.remote origin`

`branch.master.merge refs/heads/master`

and then change it:

`$ git config branch.master.remote chef`

## Keep Master Current
Use the following steps to keep the master branch up to date.

* Run:

`$ git checkout master`

* And then run:

`$ git pull --rebase`

The following `rakefile` can be used to update Chef, Ohai, and cookbooks.
Edit as necessary:

```
projects = %w[chef cookbooks ohai]
chef = "#{ENV['HOME']}/projects/chef"

desc 'Update local repositories from upstream'
task :update do
  projects.each do |p|
    Dir.chdir('#{chef}/#{p}') do
      sh 'git fetch chef'
      sh 'git rebase chef/master master'
    end
  end
end
```

## Sync Master
Use the following steps to synchronize the master branch.

* Run:

`$ git fetch chef`

* And then run:

`$ git rebase chef/master master`

#### Note
Use `rebase` instead of `merge` to ensure that a linear history is
maintained that does not include unnecessary merge commits.
`rebase` will also rewind, apply,
and then reapply commits to the `master` branch.

## Use Branch
Commits to the Chef repositories should never be made against the master branch.
Use a topic branch instead.

A topic branch solves a single and unique problem and often maps
closely to an issue being tracked in the repository.
For example, a topic branch to add support for a new init system
or a topic branch to resolve a bug that occurs in a specific version of CentOS.

Ideally, a topic branch is named in a way that associates it
closely with the issue it is attempting to resolve.
This helps ensure that others may easily find it.

Use the following steps to create a topic branch:

* For a brand new clone of the Chef repository
(that was created using the steps listed earlier), fetch the opscode remote:

`$ git fetch chef`

* Create an appropriately named tracking branch:

`$ git checkout --track -b CHEF-XX chef/master`

Set up a topic branch to track chef/master.
This allows commits to be easily rebased prior to merging.

* Make your changes, and then commit them:

`$ git status`

* And then run:

`$ git commit <filespec>`

* Rebase the commits against `chef/master`.
After work in the topic branch is finished,
rebase these commits against the upstream master.
Do this manually with `git fetch` followed by a `git rebase`
or use `git pull --rebase`.

git will let you know if there are any problems.
In the event of problems, fix them as directed,
and then mark as fixed with a `git add`,
and then continue the rebase process using `git rebase --continue`.

For example:

`$ git fetch chef`

followed by:

`$ git rebase chef/master CHEF-XX`

Or:

`$ git pull --rebase`

* Push the local topic branch to GitHub:

`$ git push origin CHEF-XX`

* Send a GitHub pull request for the changes,
and then update the Chef ticket with the appropriate information.

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

`$ git branch -d CHEF-XX`

Or remove the remote branch by using the full syntax to push`
and by omitting a source branch:

`$ git push origin :CHEF-XX`
