# GitFlowLab
A simple lab to help people get comfortable with the basic Git workflow.

This is still a work in progress, below is my TODO list:
#TODO - Add instructions for forking repo
#TODO - Split Git Ignore section off into a module
#TODO - Add section on merging
#TODO - Add module demoing how to revert a commit
#TODO - Add section on standard workflow with links to Gitflow/Trunk workflows

## Getting Started
#TODO - Add instructions for forking repo

## Git Commands and What They Actually Do
To start off, why don't we go over the basic git commands and what they actually do.  We're only really going to cover the basic usage of the commands, but be aware there are typically additional flags and options which expand on these.

#### Git Clone
Command: `git clone <url>`

Git clone retrieves the repo at the target URL and copies the contents into a directory matching the repo name in your current directory.  In addition to that, it creates an "origin" remote which points back to the url used to create the clone.

#### Git Remote
Command: `git remote` - Lists remotes

Command: `git remote add <repo url>` - Adds a new remote pointing to the \<remote url\>

Command: `git remote rename <old> <new>` - Renames the \<old\> remote to \<new\>

command: `git remote remove <remote>` - Removes a remote

The most common use for our remotes are going to be pointing back to the origin of our repo.  For example, if you clone a repo off of GitHub, then the "origin" remote should point back to the GitHub repo you cloned it from.

You may also use multiple remotes if you the repo is being stored in multiple locations.  One possible scenario would be if there are multiple forks of the repo you want to push to.  If you and a team mate had both created a fork of a repo and you wanted to push your changes to their fork, then you would create a new remote pointing to their repo.

#### Git Branch
Command: `git branch` - Lists local branches

Command: `git branch -D <branch>` Deletes local branch

Command: `git branch <branch>` Creates a new branch based on your current branch

Branches are an important concept for organizing our work.  They're a way for us to perform work in an isolated space where we can make changes without impacting other work.  If we imagine our commit history for our main branch as a straight line...
```
Initial Commit     Head
       o ----------- o
```
Now imagine that you want to make a change.  Let's say you want to add a new component.  Let's say we create a new branch called "new-component" based out of our main branch and we commit a couple of changes to our new-component branch.
```
                       new-component                     Head
                              ----- o --------- o -------- o
main                        /     
       o ----------- o ----
```
Now let's say we ran into an issue with our new-component and the code in that branch isn't working.  Another developer could still come in and push a commit to our main branch without any issue.
```
                       new-component                  
                              ----- o --------- o -------- o
main                        /     Head
       o ----------- o ------------ o
```
Because our main and new-component branches advance independently, the commits made to new-component and the commits made to main have no impact on eachother.  Even though our code is broken in the new-component branch, the code in main functions just fine.

Now we can consider two scenarios.  Let's say we realized what we were trying to do in new-component wasn't going to work, so we want to revert those changes.  All we have to do is delete the new-component branch.
```
main                              Head
       o ----------- o ------------ o
```
Alternatively, let's say we managed to get new-component working and we wanted to bring those changes over to main, we can merge the two branches.
```
                       new-component                   
                              ----- o --------- o -------- o --
main                        /                                   \  Head
       o ----------- o ------------ o ------------------------------ o
```
Now the changes that were made in our main branch and the changes in our new-component branches have been merged together and co-exist in our main branch.

In a repo that's being worked on with multiple engineers, the process of merging branches usually occurs via a pull request made in whatever version control system your team is using.  This gives other team members an opportunity to review your changes before they're merged.

How you'll utilize branches is likely going to depend on your team's workflow.  It's always a good idea to synch up with your team about when you should be creating new branches and how you'll interact with them so that everyone's on the same page.

Generally speaking though, most (if not all) of your changes should be occurring on a branch.  That way your changes are isolated, can be reviewed via a pull request, and are logically separated by specific changes which you can reject or approve individually.

#### Git Checkout
Command: `git checkout <branch>` - Switches to the target branch, updating your working files to match what's recorded in that branch.

Command: `git checkout -b <branch>` - Creates a new branch using the branch name you input and switches to it.  The new branch is based on the branch you were on when you performed the command.

Git checkout is a fairly simple command.  It's how we'll move between branches.  We can also use the '-b' flag to create a new branch and switch to it all in a single command.  One thing to note is that if you have uncommitted changes to your working files when you change branches you can bring those changes along and commit them in the new branch.  This does require that both branches have the same version of those files prior to using git checkout, otherwise you'll get an error indicating you need to commit the changes or revert those files before using git checkout.

#### Git Pull
Command: `git pull <remote> <branch>`

Pulls the contents of the target branch at the target remote into your current branch and attempts to merge them.

The most important aspect to remember is that git will attempt to merge the branch you pull into your current branch.  If you're just pulling the latest changes to your current branch, that's a pretty painless process.  However, if you're pulling another branch into your current branch, you may have merge conflicts which will need to be addressed before the merge is completed.  When this happens, the updated files will be staged to be committed, but the merge commit won't complete automatically.  The output from your pull will indicate which files have a merge conflict.  Once you've resolved those merge conflicts, those files should appear as unstaged changes.  Stage the files and then commit your changes and you should be all set.

#### Git Push
Command: `git push <remote> <branch>`

Updates the remote with the commits from the target branch.  Note that the target branch is also the branch that will be updated on the remote.  So `git push origin new-feature` would update the "new-feature" branch on the remote with the changes from your local "new-feature" branch.

If the remote contains work that you do not have, such as if someone else had pushed a commit to that branch which you haven't pulled down yet, the push will fail.  You'll need to pull the changes down, merge them with your own and then push your work up to the remote again.

#TODO - Add section on merging

#TODO - Add section on standard workflow with links to Gitflow/Trunk workflows

#TODO - Add module demoing reverting a commit

#TODO - Split Git Ignore section off into a module
## Git Ignore File
Sometimes we're working with files that contain sensitive information.  We also often use tools which create local config files.  Maybe we've got a bunch of large modules that were installed by something like NPM?

Inevitably, we're going to run into a case where we have files that we don't want added to our repo.  That's where the .gitignore file comes in!

### Ignoring files before they're tracked

#### Ignoring specific files

Let's go ahead and start by creating a new junk file that we really don't want added to our repo.

In your terminal, type:
`echo 'This is a junk file' > junk.txt`

This will create a file named "junk.txt" in our repo.

Let's go ahead and make sure that it's listed as untracked by Git.

`git status`

We should see junk.txt listed as an untracked file.  Now let's go ahead and add junk.txt to our .gitignore file.

1. Open .gitignore
2. Add the line `junk.txt` into our .gitinore file and then save the file.

Awesome, now let's verify that junk.txt is no longer showing up when we check our status.

`git status`

If everything went as we expected, then status should no longer show junk.txt as a tracked file.  Additionally, if you're using VS Code as your editor, you should see junk.txt listed as a darker shade, indicating it's being ignored.

#### Using wildcards in .gitignore

In our above example, we ignored a specific file.  However, it's entirely possible we're going to want to ignore all files of a particular type, or files that are programatically generated which we may not know the full name of.  Thankfully, .gitignore supports wildcards, so we can leverage that to help ensure we're ignoring the files we want to even if we don't explicitly add them to .gitignore.

A great example of this would be if we've place our PEM keys in our repo.  That's not information we want to share with the world, so let's go ahead and just add a line to .gitignore to ensure we're safe.

1. Open .gitignore
2. Add the line `*.pem` to our .gitignore file and then save the file.

There we go!  That should tell git to ignore any files that end with .pem.  Why don't we test it?  Thankfully, we have a handy SuperSecretInfo.pem file already on hand.  Let's make a quick change to the file and see if git picks it up.

1. Open SuperSecretInfo.pem and add "ignore me" anywhere in the file.
2. Save the file.
3. Type `git status` in your terminal

If SuperSecretInfo.pem shows up as a modified file in your `git status`, don't worry!  You haven't done anything wrong.  The .gitignore file only prevents git from tracking files if they aren't already being tracked.  Because our .pem file was already in the repo when you cloned it, then it's already being tracked by git.  This leads us to a common issue people run into with .gitignore, forgetting to ignore a file before it's committed.

### Ignoring files that are already being tracked

Alright, so we've got a file that we want git to ignore, but it's already being tracked by git.  How do we fix this?  The solution generally depends on whether or not the file contains sensitive information that we really don't want on github.  Even if we stop git from tracking the file in our next commit, the file already exists in our commit history which means it's still accessible.  Thankfully, there's a solution.  Let's start with the easy fix first though.

#### Untracking and ignoring non-sensitive files.

Alright, so our SuperSecretInfo.pem file isn't sensitive, it's just junk and we don't really want it in our repo anymore.

Thankfully, there's a quick command we can invoke that will un-track the file for us.  That should then allow our .gitignore to prevent git from tracking it on future commits.

`git rm --cached <file>`

Alternatively, if we want to untrack everything inside a folder, we can use:

`git rm -r --cached <folder>`

This will recursively remove all the files in the folder from git's cache.

Note that if there are any pending changes to these files, you will lose those changes.  If we want to keep them, commit the changes first.  In our case, this doesn't matter, but it could be a costly setback in the real world.

So, let's give it a shot!

1. Type `git rm --cached SuperSecretInfo.pem` into your terminal
2. Type `git status`
3. We should see `deleted:    SuperSecretInfo.pem` listed as a staged change.  Let's commit that change.
4. Type `git ci -m "remove pem from cache"` into your terminal
5. Type `git status`.  We should see that SuperSecretInfo.pem is no longer listed in our status!
6. Let's verify .gitignore is working and try making a change to the file.
7. Open SuperSecretInfo.pem and add `This change should be ignored` anywhere in the file then save it.
8. Type `git status` again.

There we go!  We should no longer be tracking the changes to out SuperSecretInfo.pem file.

#### Untracking and ignoring sensitive files.

Now, what do we do if we've accidentally committed a file containing sensitive information?  The answer to that depends on the circumstances.  We have a couple of routes we could take.  One of the first things we want to do is identify when the commit occurred that added the sensitive file.  You can do this by using the commit hashes in your `git log` to move through your repo and find where the file was initially committed.  Once you've got that information, you can make a decision about how to proceed.

**Create a new branch from before the file was committed**
- If the file was initially committed as a part of the work in your current branch, this may be an option.  Note that the viability of this depends heavily on how recently the commit occurred and how much work you'll lose in the process.
- You can use `git log` and `git checkout <commit hash>` to navigate through your commit history and create a new branch based out of the commit right before your sensitive file was committed.  This can help reduce the amount of work you need to recreate.
- This option's really only a good choice if the commit is recent.

**Rewrite your commit history**
- There can be serious consequences if you rewrite your commit history.  This is especially true if you're working out of a large repo or you've got multiple contributors.  Make sure you fully understand what's going to happen before you pursue this option.
- You can utilize `git filter-branch`, more info can be found [in the documentation](https://git-scm.com/docs/git-filter-branch#_exampleshttps://git-scm.com/docs/git-filter-branch#_examples)
- You can also utilize third party plugins such as git-filter-repo.  This is generally recommended over filter-branch but requires additional installation.  More info [here](https://github.com/newren/git-filter-repo/)
