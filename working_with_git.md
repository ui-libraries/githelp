# Working With Git #

[Introduction to Git](#intro)
[Setting up a depot repository](#setuprepo)  
[Move an existing repo into a depo](#moverepo) 
[Pull a repo from GitHub](#pullgithub) 
[Creating the first local working repository](#localrepo)  
[Adding a local working repository](#addlocalrepo)  
[Workflow](#workflow)  
[Committing changes](#committing)  
[Branching](#branching)  
[Merging](#merging)  
[Merging the develop branch into the master branch](#merging-develop)  
[Resources](#resources)


### <a id="intro"></a>Introduction to Git

Git is a tool for tracking different versions of software, and is designed to support collaborative development.

**Git Master**

If you haven't already, your team should assign a `git master`.  The `git master` is the person on your team tasked with managing the Git repository.  They will have special tasks to perform during the development process, and should be knowledgable about Git.

**Repositories**

A git `repository`, or `repo`, is a database containing all the information needed to retailn and manage the revisions and history of a project.  The `depot` is the authoritative repository for your project.  It will live on GitHub, and the `git master` will maintain a copy of it in /l/librarydata/dls/git.

In addition to the `depot`, each team member will have a local working repository.  This local working repo is a `clone`, or duplicate, or the `depot`.  Each work day, you will `pull` the current code from the `depot` into you local working repo.  At the end of the workday (or when you completed a significant bit of code), you will `push` your local working repo into the `depot`.  This way, everyone on your team will always be working off the same code base.  And if there are conflicts -- meaning, team members have created code that contradicts one another -- Git will help you find and fix them.

**Branches**

Branches are independent strands of development.

Your repository will have at least two branches, `master` and `develop`. The `master` branch is always a mirror of our stable production version.  It represents stable, clean, working code.  You will use the `master` branch to track major changes, or versions.  The `git master` is the only person who should make changes to the `master` branch, so if you're not the `git master`, you don't have to worry about the `master` branch. 

The `develop` branch is where you'll be sharing your code with each other while you're in development.  `develop` is dynamic -- all developers contribute to this branch.

**Index**

The `index`, or `staging area`, is an intermediate area between the files stored on your local machine and the repository.  You can think of the `index` as a set of prospective or intended modifications.  As you are working, you add, remove, move, or edit files in the index.  When you are ready, you `commit` a group of changes.

**Commits**

A `commit` is used to record changes to the repository.  You can think of it as a snapshot of the index.

### <a id="setuprepo"></a>Setting up a depot repository

If you're starting a new project, one person on your team should first set up an authoritative repository, or `depot`, in /l/librarydata/dls/git. After the `depot` has been created, each team member can [create a local working copy](#localrepo) for themselves wherever they'd like.  If you already have a git repository, you can [move an existing repo into a `depot`](#moverepo).

To set up the `depot`:

**1)**  Create a new directory for your project in /l/librarydata/dls/git. It is best practice for bare repositories to be named with `.git` suffixes.

**2)**  Open at git bash from your new directory.  You can do this by right-clicking on the directory, and selecting "Git Bash Here".

**3)**  Create a new bare repository:

	git init --bare 

Next, [create a local working copy](#localrepo).



### <a id="moverepo"></a>Moving an existing repo into a depot

If you have an existing git project you need to move a `depot`, you'll need to clone your working copy to /l/librarydata/dls/git, remove the origins, then assign this new depot repository as `origin` on your working copy.

**1)**  If you haven't already, set up your global configs.  This information will be used with all the commits you make, and will be available to other developers with access to the repositories, so consider your own privacy needs when choosing your username and email address.

	git config --global user.name "[your name]"
	git config --global user.email "[your email]"

You can see if this worked with these two commands:

	git config user.name
	git config user.email

**2)** Add this file to your repository as a `.gitignore` file.  First, put a copy of this file into your local directory.  

If you already have a .gitignore file, add this file to its rules:

	echo "working_with_git.md" >> .gitignore //make sure you type ">>" and not ">".

If you don't alreay have a .gitignore file, make one.  The `.gitignore` file tells git to ignore certain files in your local directory.  You'll want to add local instructions (such as this file), and files containing senstive information (such as passwords) to `.gitignore`.  From the master branch, create a `.gitignore` file that contians the name of this file:

	echo "working_with_git.md" > .gitignore 

If you have other files to add to `.gitignore`, add them with this command:

	echo "[filepath]" >> .gitignore  //make sure you type ">>" and not ">".

Finally, stage and commit both this file and `.gitignore`:

	git add working_with_git.md .gitignore
	git commit -m "message briefly describing change"

**3)**  Create a new directory for your project in /l/librarydata/dls/git. 

**4)**  Open at git bash from your new directory.  You can do this by right-clicking on the directory, and selecting "Git Bash Here".

**5)**  Create a bare clone of your working git repository, and rename it.  It is best practice for bare repositories to be named with `.git` suffixes:

	git clone --bare [path to your existing git repository] [new name]

**6)**  If this was successful, you should see a new directory in /l/librarydata/dls/git with the new name. This new master repository, however, thinks that your working copy is the `depot` source code. Going forward, we want this to work the other way around. You'll need to remove the origins from the new depot repository. 

From the git bash of the new `depot` repository:

	git remote rm origin

**7)** While you're here, let's set up our `master` and `develop` branches. `master` is always a mirror of our stable production version. `develop` is where we'll be sharing our code with each other while we're in development.

	git branch develop

If you have any extra branches from your working copy that were cloned over, you should delete these (assuming your most stable working code is in the master branch).

**8)**  Now let's go back to your working copy and assign the new depot repo as your working copy's `origin`. Open a git bash at your working copy's directory, and add a remote called `origin`:

 	git remote add origin /l/librarydata/dls/git/[depot_repository_name]

 You can see if this worked with the command:

 	git remote show origin 

You should see the `depot` repository and its branches.

Congratulations - you have moved your existing git repository into a `depot`, and set up your local working repository its original location!  From now on, you can follow the standard [workflow](#workflow).


### <a id="pullgithub"></a>Pull an existing repo from GitHub

If you have an existing git project already stored in GitHub, you'll need to pull it to your working repository.  Before you begin, make sure that you have a GitHub account, and that whomever intitiated the GitHub repository has added you as a collaborator.  If you haven't already, you may need to set up a secure SSH key.  For instructions, see [https://help.github.com/articles/generating-ssh-keys](https://help.github.com/articles/generating-ssh-keys).

**1)**  Create a new directory for your project anywhere you'd like.  Open at git bash from your new directory.  You can do this by right-clicking on the directory, and selecting "Git Bash Here".

**2)**  Initiate your new working repositgit ory.

	git init

**3)**  If you haven't already, set up your global configs.  This information will be used with all the commits you make, and will be available to other developers with access to the repositories, so consider your own privacy needs when choosing your username and email address.

	git config --global user.name "[your name]"
	git config --global user.email "[your email]"

You can see if this worked with these two commands:

	git config user.name
	git config user.email


**4)**  Make the GitHub repo the origin of your working repo.  Here is an example of the format of a GitHub repo name:  git@github.com:ui-libraries/awesome-project.  

	git remote add origin [GitHub repo]

**5)**  Pull the GitHub repo into your working repo.

	git pull origin master  

Congratulations - you now have a working repo whose origin is the GitHub repository.  From now on, you can follow the standard [workflow](#workflow).


### <a id="localrepo"></a>Creating the first local working repository

These instructions will tell you how to set up the *first* local working repository for your project, and push the intital necessary content to the `depot`.  If someone else on your team has already set up a local working repository, please see [Adding a local working repository](#addlocalrepo).

To create a local working repository, you'll simply clone the `depot` repository.  

**1)**  Create a new directory for your working repository anywhere you'd like. 

**2)**  Open at git bash from your new directory.  You can do this by right-clicking on the directory, and selecting "Git Bash Here".

**3)**  Clone the depot.  If you want to rename it, you can type in a new name, but this is optional.  It is best practice for *bare* repositories to be named with `.git` suffixes.  Since this is *not* a bare repository, it should *not* be named with a `.git` suffix.

	git clone [path to depot repository] [new name]

If this was successful, you should see the new working directory in the place you've chosen.

**4)** Set your user name and email address.  This information will be used with all the commits you make, and will be available to other developers with access to the repositories, so consider your own privacy needs when choosing your username and email address.

	git config --global user.name "[your name]"
	git config --global user.email "[your email]"

You can see if this worked with these two commands:

	git config user.name
	git config user.email

**5)** Stage any new files.  First, move a copy of this file to your repository directory (the one you made in step 1).  If you already have additional files to add, move them to your repository directory as well.  

If you aren't there already, navigate to the working repoistory you created in step 3.  You can open a new bash by right-clicking on the directory, and selecting "Git Bash Here", or in your open bash, use this command:

	cd [filepath to working repository]

Stage each file to the develop branch:

	git add [filepath]  //you can list more than one file

To see if this worked, type:

	git status

You should see all the files you want to add listed under "Changes to be committed".

**6)** Make a `.gitignore` file.  The `.gitignore` file tells git to ignore certain files in your local directory.  You'll want to add local instructions (such as this file), and files containing senstive information (such as passwords) to `.gitignore`.  First, create a `.gitignore` file that contians the name of this file:

	echo "working_with_git.md" > .gitignore 

If you have other files to add to `.gitignore`, add them with this command:

	echo "[filepath]" >> .gitignore  //make sure you type ">>" and not ">".

Stage `.gitignore` to the develop branch:

	git add .gitignore

**7)** After you have staged all of your files, commit them to the index (you only have to do this once to commit all of the files you've added):

	git commit -m "Message briefly describing change"  

To see if this worked, check the list of commited files.  Your file should be listed.

	git ls-files

If you need more help with staging and committing files, see [Committing changes](#committing).

**8)** Set up the `master` and `develop` branches. `master` is always a mirror of our stable production version. `develop` is where we'll be sharing our code with each other while we're in development.  Git automatically creates a `master` branch for you, but you'll need to create a `develop` branch:

	git branch develop

To see if this worked, list all the branches in your repository:

	git branch

The develop branch should be in the list.


**9)** Finally, push the `master` and `develop` branches to the `depot` (in your git bash, the word "origin" refers to the `depot`).  This is a very important step, because it updates the `depot` to be in sync with your working repository.

	git push origin master
	git push origin develop

Congratulations - you now have a fully-functioning working repository, and you have pushed the inital necessary content to the `depot`!  From now on, you can follow the standard [workflow](#workflow).


### <a id="addlocalrepo"></a>Adding a local working repository

Follow these instructions if someone on your team has already created the first local working repository, and pushed the necessary initial content to the `depot`.  If this is not the case, see [Creating the first local working repository](#localrepo).

**1)**  Create a new directory for your working repository anywhere you'd like. 

**2)**  Open at git bash from your new directory.  You can do this by right-clicking on the directory, and selecting "Git Bash Here".

**3)**  Clone the depot.  If you want to rename it, you can type in a new name, but this is optional.  It is best practice for *bare* repositories to be named with `.git` suffixes.  Since this is *not* a bare repository, it should *not* be named with a `.git` suffix.

	git clone [path to depot repository] [new name]

If this was successful, you should see the new working directory in the place you've chosen.

**4)** Set your user name and email address.  This information will be used with all the commits you make, and will be available to other developers with access to the repositories, so consider your own privacy needs when choosing your username and email address.

	git config --global user.name "[your name]"
	git config --global user.email "[your email]"

You can see if this worked with these two commands:

	git config user.name
	git config user.email

Congratulations - you now have a fully-functioning working repository!  From now on, you can follow the standard [workflow](#workflow).


### <a id="workflow"></a>Workflow

Before you begin, there should be an existing authoritative repository, or `depot`, for your project in l://librarydata/dls/git, or you should have an authoritative repostitory in GitHub. If not, see [Setting up a depot repo](#setuprepo) or [Moving an existing repo](#moverepo).  You should also have a local working repository, located anywhere you'd like.  If not, see [Creating the first working local repository](#localrepo) or [Adding a local working repository](#addlocalrepo).

Your daily workflow should look like this. From your local working repo:

**1)** Open at git bash from your new directory.  You can do this by right-clicking on the directory, and selecting "Git Bash Here".

**2)**   Pull back any changes from origin/develop, so you're in sync with the most current codebase.  

First, if you're not there already, navigate to the develop branch: 

	git checkout develop 

Pull the latest code from the develop branch of `depot`: 

	git pull origin develop 

If there are merge conflicts, you can use the `git diff` command to see which file are in conflict with one another, and the differences between them.  Run `git status` to see which files are unmerged at any point after a merge conflict.  Once you have made appropriate changes to files, you will need to [commit the changes to the repository](#committing).

**3)**  If you are doing your work on a branch of your local working repo besides develop, update this branch as well: 

	git merge [name of the branch you are working on]

**4)**   Do your work, commit often, rinse, repeat.  If you need help committing, see [Committing changes](#committing).

**5)**   When you're ready (or when you've come to a good stopping place), push your code to the `depot` develop branch.  Please do this often, but **make sure you have working code** when you do.

If you've been working on a branch other than develop, go back to the develop branch:

	git checkout develop

Pull the latest code from origin.  This is very important - someone may have pushed new changes to the origin since you've started working!

	git pull origin develop

Merge your code back into updated develop branch: 

	git merge [name of the branch you are working on]  //this merges your code into the develop branch

Push your code to the `depot`: 

	git push origin develop

If there are merge conflicts, you can use the `git diff` command to see which file are in conflict with one another, and the differences between them.  Run `git status` to see which files are unmerged at any point after a merge conflict.  Once you have made appropriate changes to files, you will need to [commit the changes to the repository](#committing).

**4)**   Go home. Come back tomorrow and start with step 1.



### <a id="committing"></a>Committing changes

There are two steps to commiting files to git.  First, you will stage files from your local directory to the index.  Second, you will commit files from the index to the repository.  The index is a set of intended modifications, which you can change right up to the culminating commit.  When you commit, Git checks everything in the index, rather than the local directory, to discover what to commit.  

**1)**  Open up git bash at your working repository, and navigate to the directory to which to want to add a file.

**2)**  Add (aka stage) a file to the index:

	git add [filepath]  // can list more than one file

To see if this worked, check the status of the index:

	git status

You should see all of the files you want to add listed under "Changes to be committed".  The git status command shows you all of the files in your local directory, and their relationship to the working directory.  You can use it at any time to see whether or not each file has been staged to the index, and if each file is new, modified, or untracked (was not committed in the previous commit).

**3)**  Commit all files in the index to the repository:

	git commit -m "Message briefly describing change"

Your username and email will be automatically attributed to this commit.  If you have not yet set your git identity, [set up your configs](#configs).

To see if this worked, check the list of commited files.  Your file should be listed.

	git ls-files

**4)**  If your commit represents a significant accomplishment, or the completion of a major task, you should tag it.  The tag is a "stake in the ground" that will help you and your fellow developers track changes to your code.

	git tag -a [tag name] -m '[brief message describing tag]'  

To see if this worked, list all the tags.  Your new tag should be at the end of the list.

	git tag 

Below are some additional commands that can be helpful when committing changes.

Unstage a file:

	git reset [filepath]

Remove file from both the index and local directory:

	git rm [filepath] 

Remove a file from the repository, and tell git not to track in anymore:

	git rm --cached [filepath]

Move/rename file in index:

	git mv [old filepath] [new filepath]

See the history of old commits:

	git log

See only the last two commits:

	git log -2  //you can enter any number

See commits from the last 2 weeks:

	git log --since=2.weeks  //you can enter any number

See the difference between your working directory and the index:
	
	git diff

See the difference between the index and the latest commit: 
	
	git diff HEAD

See the details of a git tag, including the author and message:
	
	git show [tag name]

Add a file to .gitignore:

	echo "[filepath]" >> .gitignore  //make sure you type ">>" and not ">".


### <a id="branching"></a>Branching

Branches are useful for opening up independent lines of development.  To create a new child branch:

**1)**  Switch to the branch you want to branch from (all repositories have at least one `master` branch):

	git checkout [branch name]

**2)**  Create a new branch:

	git branch [branch name]

The default when branching is the revision committed most recently.  If you need to start your branch from an earlier commit, use this command:

	git branch [branch name] [starting commit]

To see if this worked, list all the branches in your repository:

	git branch

Your new branch should be in the list.

**3)**  When you are ready to reincorporate your child branch into your code base, you can [merge it back into it source branch](#merging).



### <a id="merging"></a>Merging

Note: The git administrator is the only person who should [merge the develop branch into the master branch](#merging).

To merge a child branch back into its source branch:

**1)**  Switch to the source branch.
	
	git checkout [source branch name]

**2)**  Merge the child branch back into the source branch:

	git merge [child branch name]

If there are merge conflicts, you can use the `git diff` command to see which file are in conflict with one another, and the differences between them.  Run `git status` to see which files are unmerged at any point after a merge conflict.  Once you have made appropriate changes to files, you will need to [commit the changes to the repository](#committing).

**3)**  When you are completely done with a branch, delete it:

	git branch -d [branch name]

To delete a branch from the `depot`:

	git push origin :[branch name]

You can also periodically merge the source branch into the child branch, to keep the child branch in sync with the changes in the source branch.  To do this:

**1)**  Switch to the child branch:

	git checkout [child branch name]

**2)**  Merge the spcecial-project branch back into the source branch:

	git merge [source branch name]



### <a id="merging-develop"></a>Merging the develop branch into the master branch

When the team has reached a milestone the development process, the git administrator will merge the `develop` branch into the `master` branch, and push this change to the `depot`.  

**1)** Make sure that all team members have pushed their working code to the `depot`, and that the code has been tested to ensure that everything works.

**2)** Open at git bash from your local working directory.  You can do this by right-clicking on the directory, and selecting "Git Bash Here". 

**3)**  Pull back any changes from origin/develop, so you're in sync with the most current codebase: 

If you're not there already: 

	git checkout develop  

Pull the latest code: 

	git pull origin develop 

**4)**  Switch to the `master` branch:

	git checkout master

**5)** Merge `develop` into `master`:

	git merge develop

If there are merge conflicts, you can use the `git diff` command to see which file are in conflict with one another, and the differences between them.  Run `git status` to see which files are unmerged at any point after a merge conflict.  Once you have made appropriate changes to files, you will need to [commit the changes to the repository](#committing).

**6)** Push the `master` branch to the `depot`:

	git push origin master

**7)** Create a tag to mark your milestone and push it to the `depot`:

	git tag -a [tagname] -m "[message describing milestone]"
	git push origin [tagname]

Huzzah!  You have a milestone in your development process by merging it with the master branch, and pushing it to the `depot`.  Don't forget to also push the develop branch to the `depot` if you have made changes to it during this process.  

###<a id="resources"></a>Resources

[Git official documentation](http://git-scm.com/documentation)
[Atlassian Git Tutorials](https://www.atlassian.com/git/tutorial)