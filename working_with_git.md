### <a id="setuprepo"></a>Setting up a master repository

If you're starting a new project, you should first set up a repository for it in our source code repository at /l/librarydata/dls/git. After you've created this master repository, you can clone a working copy for yourself wherever you'd like.

**1)**  Create a new directory for your project in /l/librarydata/dls/git

**2)**  Open at git bash from your new directory

**3)**  Create a new bare repository:

	git init

**4)**  If you already have files to add, move them to your repository directory and [commit](#committing) them to git. If not, then add a copy of this .gitignore file and commit it before creating your local working copy of the repository

Next, [create a local working copy](#localrepo).


### <a id="moverepo"></a>Moving an existing repo to master

If you have an existing git project you need to move to our master source code repository, you'll need to clone your working copy to /l/librarydata/dls/git, remove the origins, then assign this new master repository as `origin` on your working copy.

**1)**  Open a git bash at /l/librarydata/dls/git

**2)**  Clone your working git repository:

	git clone [path to your existing git repository]

**3)**  If this was successful, you should see a new directory in /l/librarydata/dls/git with the same name as your working copy. This new master repository, however, thinks that your working copy is the master source code. Going forward, we want this to work the other way around. You'll need to remove the origins from the new master repository. From the git bash of the master repo:
	git remote rm origin

**4)** While you're here, let's set up our `master` and `develop` branches. (In our master source code repository, `master` is always a mirror of our stable production version. `develop` is where we'll be sharing our code with each other while were in development.):

	git branch develop

If you have any extra branches from your working copy that were cloned over, you should delete these (assuming your most stable working code is in the master branch).

**5)**  Now let's go back to your working copy and assign the new master repo as your working copy's `origin`. Open a git bash at your working copy's directory, and add a remote called `origin`:

 	git remote add origin /l/librarydata/dls/git/[master_repository_name]

 You can see if this worked with the command:

 	git remote show origin //this should show you the repository and branches assigned to origin

Next, [set up your configs](#configs).


### <a id="localrepo"></a>Creating a local working repository

to-do

### <a id="configs"></a>Setting up your configs

to-do

### Workflow

Before you begin, there should be an existing master repository for your project in l://librarydata/dls/git. If not, see [Setting up a master repo](#setuprepo) or [Moving an existing repo](#moverepo).

After you've created a local working repo, your daily workflow should look like this. From your local working repo:

**1)**   Pull back any changes from origin/develop, so you're in sync with the most current codebase: 

If you're not there already: 

	git checkout develop  //this will put you in the develop branch of your local working repo

Pull the latest code: 

	git pull origin develop // this pulls and merges the current code from the 'develop' branch of our master repo

If there are merge conflicts:

	instructions go here

If you are doing your work on another branch of your local working repo, do: 

	git merge [working_repo_here]

**2)**   Do your work, commit often, rinse, repeat.

**3)**   When you're ready (or when you've come to a good stopping place), push your code to the master development branch (please do this often, but make sure you have working code when you do):

If you've been working on a separate branch than develop: 

	git merge develop //this merges your code with your working repo's main branch

Make sure the origin/develop branch is not checked out (if it is, then just checkout the master branch).

Push to origin: 

	git push origin develop

If there are merge conflicts:

	instructions go here

**4)**   Go home. Come back tomorrow and start with step 1.

### <a id="committing"></a>Committing changes

to-do
