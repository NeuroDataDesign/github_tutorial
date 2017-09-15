# Github/Git Overview

This tutorial will be an overview of some of the basic features of github and git. In the README, we will see some basic functions one might need from git, along with applications of the respective functions geared towards a beginner. Note that the use-cases here will be focused on first-time users, so a number of functionality discussed (such as `git reset`) will be discussed in the context of a new user who may not understand more complex `git` manipulations. 

# Cloning an existing repo

We will begin by assuming the user has configured a repository through git, and initialized it with something like a README using the github website GUI (to initialize a repository from the command line, see the tutorial [here](http://kbroman.org/github_tutorial/pages/init.html), though this is a more complex feature). A user can clone a "remote" repository (remote in CS terms means a repository that is hosted by a service like github or bitbucket) to "local" ("local" in CS terms is a file/directory that exists on your computer) as follows:

```
git clone <address>
```

where `<address>` can be obtained from the repository service you are using. In github, for example, the address can be found in the GUI as follows under the green button, and then clicking for "HTTPS":

![image](https://user-images.githubusercontent.com/8883547/30497058-5d838bdc-9a1f-11e7-9120-5f9962cf7a46.png)

For information on using SSH keys, which simplify the git interface (in that they allow you to not type a password over and over again) you can follow the tutorial [here](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/), but we won't cover that here. Now that we have the repository locally, we can cover some of the basics.

# Pulling from a Remote

Before you do anything when you are at your repo, the number 1 command to always use is the pull command. The pull command can be invoked as follows from your terminal:

```
git pull
```

"Pulling" essentially allows you to take any changes that have occurred remotely (such as from another user updating the remote), and adds them to your local copy of the repository. Using (or abusing) this command will ensure that your repository stays as close to in-sync with the remote repository as possible, which will greatly simplify your life down the line. 

# Checking the Status of your Local Copy

When you make changes to your repository, you will be able to see these changes in the context of the remote's status by invoking the status command. You can use this command as follows:

```
git status
```

Again, this is a command to get extra comfortable with, and use (and abuse) over and over again as a git user. This will tell you which files exactly you have changed, and when you try to update the remote yourself (later on) you will know exactly which files you have touched locally and will need to update. 

# Adding, Committing and Pushing your Changes

When you have made local changes to a branch (ie, you have updated a file locally), you will want to update the remote with your local updates. The sequence for updating the remote branch is as follows:

```
git status  # check what files you updated
git add path/to/file1.ext path/to/file2.ext  ... # add all the files you meant to change that were shown by git status
git commit -m 'insert a descriptive message of what was changed'
git push  # your changes will be updated in the remote
```

Generally, if you are only adding new files, this procedure will work just about 100% of the time. This is because as long as you are the only person touching the files, you should end up with few/no conflicts when you try to commit your changes. if you receive an error message when typing `git commit` or `git push`, there are plenty of ways to debug (check google or stack exchange if you are curious), but if you are new to git, you should begin by asking your team leader what to do to avoid messing things up, as git issues can get messy very quickly! 

# Making a new Branch

When adding a significant feature, it is important to make a feature branch. This is because it is often vital to have a working copy of the entire project, while adding the feature iteratively (and potentially breaking downstream functionality temporarily in the process). Benefits will also be that you will be able to push your changes and store them remotely without needing to only make one long commit with a bunch of changes that may or may not be sufficiently vetted. To make a new branch, you may first want to verify the current branch you are on with the following command:

```
git branch
>> master*
```

Which will report back your current working branch with the *. To make a new branch, we can invoke the following command:

```
git checkout -b <new-branch-name>
```

When naming new branches, it may be useful to tag them as `username-dev` or `feature-###-dev` to make it easier to filter the branches later on. Once we have checked out our new branch, we can push it to the remote with:

```
git push origin <new-branch-name>
```

Now, you can switch back and forth between your existing branches:

```
git branch 
>> new-branch-name*
>> master
got checkout master
>> new-branch-name
>> master*
```

and your local files will be updated accordingly to reflect the appropriate branch you are on. 

# Merging a branch back to master

By the time your branch is finished, you may be several commits behind the master branch (ie, somebody else may have updated the master branch while you were updating your local branch, which will create issues later on!). Fortunately, merging exists to simplify this procedure. Starting from your feature branch (ie, we will want `git branch` to return with `new-branch-name*`) we can merge in the remote changes to master to our local repository:

```
git merge master
```

This command will merge the updates from master into your local branch, either working seamlessly (if you have not changed any files that were changed from master) or not so seamlessly (if you have changed files that were changed from master). If you have touched files that have been updated by somebody else, you will see the `merge` command reporting merge-conflicts. These conflicts can be resolved manually by opening the appropriate files, finding lines with something as follows:

```
<<<<<<< HEAD
### remote copy of the area of the file that was changed are here
=============
### local copy of the area of the file that was changed will be here
>>>>>>>>  ################ (the commit id)
```

and these will be present anywhere that `git` can detect that two people updated the file in the same place. These need to be resolved manually (by selecting which lines from each function to retain or delete), and often it will be useful in this situation to ask your group leader or a more experienced git user for help if you don't know what to do (to make sure you don't accidentally delete anything useful). When you are finished manually merging, you can commit and push the changes back to your repository (following the above instructions for committing and pushing) and type `git continue` to continue merging the next file. Make sure to check `git status` when merging so that you can make sure you merge all the files successfully before pushing your changes. 

# Making a Pull Request

Once you have successfully merged master (or whatever branch you want to add your changes to) to your development branch, you may be ready to make a pull request, or a PR. You can do this through the github GUI by selecting the Pull Request tab at the top, making a new PR, and then selecting the branch you want to merge INTO as your "base", and your branch you want to add changes from in the "compare". You can make the PR, and then wait for your group leader/whomever is in charge of your repo to approve the changes into master, or request that you make further changes before it is accepted into master. If you have made significant changes in your development branch, it is good git practice to provide testing code that the maintainer can run to verify that your development branch 1) adds your developmental feature, 2) improves the target branch in some way, and 3) has not broken the target branch in your PR to greatly simplify the process. 
