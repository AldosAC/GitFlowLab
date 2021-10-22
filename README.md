# GitFlowLab
A simple lab to help people get comfortable with the basic Git workflow.

## Getting Started


## Git Ignore
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

Now, what do we do if we've accidentally committed a file containing sensitive information?  The answer to that depends on the circumstances.  We have a couple of routes we could take.  One of the first things we want to do is identify when the commit occurred that added the sensitive file.  To do this...

1. Type `git log` in your terminal.
2. This displays our commit history with a hash for each commit.  It should have entries like...
```
commit a5e6b65a49ef332df489affc1874964573165742 (origin/main, origin/HEAD)
Author: Joel Carpenter <66389984+AldosAC@users.noreply.github.com>
Date:   Fri Oct 22 11:28:16 2021 -0700

  Initial commit
```
3. The string to the right of the word "commit" is the hash for the commit.  We can use this to checkout that commit, effectively moving backwards in our commit history temporarily.
4. Type `git checkout 6638d3d11ad694ab2888c24d00986b931d958f79`.  Note that you may need to commit changes to your files before invoking this command otherwise your changes may be lost.
5. This will bring us back to the commit which added the initial files for the repo.  We're currently in a "detached head" state, so we won't be able to make permanent changes from our current state without creating a new branch.  However, we can check to see if our sensitive file is tracked.
6. 

**Create a new branch from before the file was committed**
- If the file was initially committed as a part of the work in your current branch, this may be an option.  Note that the viability of this depends heavily on how recently the commit occurred and how much work you'll lose in the process.
- You can use `git log` and `git checkout <commit hash>` to navigate through your commit history and find