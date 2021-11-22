# GitFlowLab
A simple lab to help people get comfortable with the basic Git workflow.

This is still a work in progress and will be updated periodically to finish filling out the content.  Notably, the merge conflict, git ignore, and revert commit modules are currently incomplete.

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

## What is Git and why is it relevant to us?
Great question!  At its core, git is an organization tool that helps us manage changes to our code base.  Git keeps a record of committed changes which we can use to navigate through our code's history.  We could use that to revert bad changes, view the code at a specific point in time, or any number of other uses.  In addition to keeping a commit history, Git also leverages the concept of "branches" to enable us to experiment safely and work in parallel with other developers.  All while providing a quick, simple way for us to merge our work when we're done.  There are certainly other features that Git offers, but that's really the core of what it does for us.  It provides a way for us to manage our code base safely while also trying to make working with other developers as painless as possible.

In a professional environment, we're going to leverage those features in a couple of different ways.  We will...
* Use commits to track milestones in our work with clear, concise commit messages so our team members can understand the work we've done at a glance.
* Utilize branches to experiment with changes without interrupting our coworkers who might also be working out of our repo.
* Treat branches as way to organize our changes, limiting the changes in a branch to a specific feature or fix.
* Leverage code reviews in pull requests as a way to maintain quality and consistency.
* Protect sensitive information by adding files we don't want tracked to our .gitignore file
* Resolve conflicting changes with as little headache as possible by communicating with our team as they occur.

This guide aims to help us get comfortable with that process by diving a little deeper into the common commands we use and what the different components of those commands actually do.

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
While Git does a great job of automatically merging branches, as more people contribute to the same repo merge conflicts will start to become a common occurance.  Merge conflicts arise when there are conflicting changes to the same line (or lines) of code.

If you're merging or pulling a branch on your local machine, merge conflicts will be noted in the terminal as git attempts to merge the branches.  However, if the merge conflict is detected during a pull request, it should be noted in the pull request itself.  Some version control systems support resolving merge conflicts via the browser, but you can always resolve the merge conflicts locally.

Assuming we wanted to merge a branch named `feature` into `main`, we would:
1. Open the terminal and navigate to our local repo
2. `git checkout main`
3. `git pull main` to ensure our local version of main is up to date
4. `git checkout feature`
5. `git pull origin feature` to make sure your local version of our feature branch is up to date.
6. `git merge main` to attempt to merge main into our feature branch.
7. Examine the output from git merge and note which files have merge conflicts.
8. Open your text editor and resolve the conflicts in each file with a conflict.
9. Stage the merged files and then commit your changes.
10. `git push origin feature`
11. Navigate back to the PR in your version control system and verify that it has flagged the merge conflicts as resolved.

When resolving merge conflicts, it's really critical that you're aware of what the conflicting change is and why that change was made.  If the change was made by another developer, it's a good idea to reach out in advance and make sure your proposed changes won't break anything they were working on.

Once you have an understanding of what the conflicting changes are there for, you can make the decision about whether you want to overwrite one set of changes or you want to combine your changes.  No matter the route you go, make sure you take a moment before saving and staging your merged files to ensure there aren't any syntax errors as a result of merging the changes.

If you'd like some additional practice with resolving merge conflicts, I've provided a module on merge conflicts in the `1-Merge-Conflicts` directory.  Take a look at the README there and you can practice resolving some merge conflicts.

---

## Some Common Workflows

First and foremost, it's important to understand that there a many different schools of thought on how best to utilize Git.  Everyone tends to develop personal preferences, but what's really important is identifying the workflow your team utilizes.

There are some common workflows, such as the [GitFlow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow) or [trunk-based](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow) models which are worth familiarizing yourself with.

What these models aim to do is create a common process for a team to use as they make changes.  This usually revolves around how branches are designed and managed, where changes should get pushed to, what pull requests and code reviews look like, and what commit/pull request messages should include.

When you join a new team, it can be really helpful to ask these questions:
* Is there a common workflow we use?
* When should I be creating a branch, and when should I be merging it?
* What's our pull request process?
* What branch should I be basing new branches off of?

Regardless of the model your team uses, there are some common core concepts we'll lean on.

* Any changes you make should be done in a branch.
* Try not to commit broken code.
* There's usually a core working branch that all other branches should be based off of (typically main or develop)
* Do not push changes directly to main or develop
* Branches should be merged via a pull request.
* A team member (usually a senior member) should review your code before your pull request is completed.
* All comments in a pull request should be resolved and any validation tests should be passing before a pull request is completed.
* If you run into a merge conflict, it's a good idea to communicate with the other developer who made the conflicting change before resolving the conflict.

I'm sure there are some other great ground rules for working as a team with Git!  This should really be taken as a general guideline and not a definitive list.  With that in mind, if you have recommendations or suggestions please feel free to submit a pull request for this very repo!

---

## Helpful Links
* [Git Cheatsheet: A great cheat sheet from Atlassian, covers most common commands](https://www.atlassian.com/git/tutorials/atlassian-git-cheatsheet)
* [Git - The Simple Guide: Another great, low complexity guide to using Git](https://rogerdudler.github.io/git-guide/)
* [GitFlow: An overview of the GitFlow branching model](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow)
* [Trunk-based development: An overview of the trunk based branching model](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow)
* [Git Fetch and Merge: A great article that does a deep dive on the difference between git fetch and git pull](https://longair.net/blog/2009/04/16/git-fetch-and-merge/)
* [Git Documentation: Your reference manual to git commands](https://git-scm.com/doc)
