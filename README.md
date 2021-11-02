# GitFlowLab
A simple lab to help people get comfortable with the basic Git workflow.

This is still a work in progress, below is my TODO list:
- #TODO - Split Git Ignore section off into a module
- #TODO - Add section on merging
- #TODO - Add module demoing how to revert a commit
- #TODO - Add section on standard workflow with links to Gitflow/Trunk workflows

## Getting Started
Before we dive in, let's fork this repo so you've got a safe place to experiment with Git.
From this [GitHub repo](https://github.com/AldosAC/GitFlowLab), click on the "Fork" button on the top-right of the screen.  If prompted to select a destination for the fork, select your GitHub account and proceed.  You should now have your own copy of the repo!  Congratulations, feel free to break anything in there that you want.

Now that you've got your very own fork of the repo, go ahead and clone that down to your local machine.  

To do so: 
1. Click the "Code" button on the repo and copy the "Clone" link provided.
2. Open your terminal and navigate to whatever director you want to place the repo in.
3. Type `git clone <url>` into your terminal, replacing /<url/> with the link you copied in step 1.
4. cd into your fancy new repo!

---

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

#### Git Push
Command: `git push <remote> <branch>`

Updates the remote with the commits from the target branch.  Note that the target branch is also the branch that will be updated on the remote.  So `git push origin new-feature` would update the "new-feature" branch on the remote with the changes from your local "new-feature" branch.

If the remote contains work that you do not have, such as if someone else had pushed a commit to that branch which you haven't pulled down yet, the push will fail.  You'll need to pull the changes down, merge them with your own and then push your work up to the remote again.

#### Git Pull
Command: `git pull <remote> <branch>`

Pulls the contents of the target branch at the target remote into your current branch and attempts to merge them.

The most important aspect to remember is that git will attempt to merge the branch you pull into your current branch.  If you're just pulling the latest changes to your current branch, that's a pretty painless process.  However, if you're pulling another branch into your current branch, you may have merge conflicts which will need to be addressed before the merge is completed.  When this happens, the updated files will be staged to be committed, but the merge commit won't complete automatically.  The output from your pull will indicate which files have a merge conflict.  Once you've resolved those merge conflicts, those files should appear as unstaged changes.  Stage the files and then commit your changes and you should be all set.

#### Git Merge
Command: `git merge <source branch>`

Merges the contents of the source branch into your current branch.  Make sure you read the logging as the command executes as it will note any files which had merge conflicts.  If there is a merge conflict, git will stage all of the files to complete the merge commit, but won't complete the commit.  Modify the files containing conflicts so that the conflicts are resolved, then save them and stage them to be committed.  Once all merge conflicts are resolved and everything is staged, complete the commit to finalize the merge.

If you need to cancel the merge while you're attempting to resolve merge conflicts, you can use the command: `git merge --abort`

---

## Resolving Merge Conflicts



#TODO - Add section on standard workflow with links to Gitflow/Trunk workflows

#TODO - Add module demoing reverting a commit

#TODO - Split Git Ignore section off into a module
