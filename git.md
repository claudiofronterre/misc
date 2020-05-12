# Instructions for Git

## Installation

If `git` is not installed already install it using the following code:

**Ubuntu**
```
sudo apt-get install git
```

**Mac OS**

Install [Homebrew](http://brew.sh), "the missing package manager for OS X". Among many other things, it can install Git for you. Once you have Homebrew installed, do this in the shell:

```
brew install git
```

Now we need to introduce ourself to git. In the shell type:

``` bash
git config --global user.name 'Claudio Fronterre'
git config --global user.email 'cla.f90@gmail.com'
git config --global --list
```

substituting your name and **the email associated with your GitHub account**.

The [usethis package](https://usethis.r-lib.org) offers an alternative approach. You can set your Git user name and email from within R:

```r
## install if needed (do this exactly once):
## install.packages("usethis")
library(usethis)
use_git_config(user.name = "Claudio Fronterre", user.email = "cla.f90@gmail.com")
```

Now create a new repository on github, clone it locally, make a change and then run then commit the changes and push them to the remote repository to check that eveyrhting works fine. The command line routine to commit and push is:

```
git add -A
git commit -m "my commit message"
git push
```

## Cache credentials for either HTTPS or ssh

From January 2019 there should be no need for this, it shoulw work smoothly. It is reccomended to use HTTPS credentials over ssh. If you still have problems with HTTPS credentials follow [this chapeter](https://happygitwithr.com/credential-caching.html) to set them up.

## Connect RStudio to Git and GitHub

Let's create a repository to test that RStudio can communicate with Git and GitHub. Go to <https://github.com> and make sure you are logged in. Click the green "New repository" button. Or, if you are on your own profile page, click on "Repositories", then click the green "New" button.

How to fill this in:

  * Repository name: `myrepo` (or whatever you wish, we'll delete this soon anyway).
  * Description: "testing my setup" (or whatever, but some text is good for the README).
  * Public.
  * YES Initialize this repository with a README.
  
For everything else, just accept the default. Click the big green button "Create repository." Copy the HTTPS clone URL to your clipboard via the green "Clone or Download" button.

In RStudio, start a new Project:

  * *File > New Project > Version Control > Git*. In "Repository URL", paste the URL of your new GitHub repository. It will be something like this `https://github.com/jennybc/myrepo.git`.
    - Do you NOT see an option to get the Project from Version Control? Restart RStudio and try again. Still no luck? Go to chapter \@ref(rstudio-see-git) for tips on how to help RStudio find Git.
  * Accept the default project directory name, e.g. `myrepo`, which coincides with the GitHub repo name.
  * Take charge of -- or at least notice! -- where the Project will be saved locally. A common rookie mistake is to have no idea where you are saving files or what your working directory is. Pay attention. Be intentional. Personally, I would do this in `~/tmp`.
  * I suggest you check "Open in new session", as that's what you'll usually do in real life.
  * Click "Create Project".

You should find yourself in a new local RStudio Project that represents the new test repo we just created on GitHub. This should download the `README.md` file from GitHub. Look in RStudio's file browser pane for the `README.md` file.

### Make local changes, save, commit, push

From RStudio, modify the `README.md` file, e.g., by adding the line "This is a line from RStudio". Save your changes.

Commit these changes to your local repo. How?

From RStudio:

  * Click the "Git" tab in upper right pane.
  * Check "Staged" box for `README.md`.
  * If you're not already in the Git pop-up, click "Commit".
  * Type a message in "Commit message", such as "Commit from RStudio".
  * Click "Commit".
  
Click the green "Push" button to send your local changes to GitHub. If you are challenged for username and password, provide them (but see below). You should see some message along these lines.

``` bash
[master dc671f0] blah
 3 files changed, 22 insertions(+)
 create mode 100644 .gitignore
 create mode 100644 myrepo.Rproj
```

## Workflows 

### New project, GitHub first

This is the best thing to do when you start a new project. First create the repo for the project on GitHub and then create a new R project with the version control option. Do the following once per new project:

Go to <https://github.com> and make sure you are logged in. Click green "New repository" button. Or, if you are on your own profile page, click on "Repositories", then click the green "New" button.

- Repository name: `myrepo` (or whatever you wish)  
- Public  
- YES Initialize this repository with a README

Click the big green button "Create repository." Copy the HTTPS clone URL to your clipboard via the green "Clone or Download" button. Or copy the SSH URL if you chose to set up SSH keys.

In RStudio, start a new Project:

  * *File > New Project > Version Control > Git*. In the "repository URL" paste the URL of your new GitHub repository. It will be something like this `https://github.com/jennybc/myrepo.git`.
  * Be intentional about where you create this Project.
  * Suggest you "Open in new session".
  * Click "Create Project" to create a new directory, which will be all of these things:
    - a directory or "folder" on your computer
    - a Git repository, linked to a remote GitHub repository
    - an RStudio Project
  * **In the absence of other constraints, I suggest that all of your R projects have exactly this set-up.**

This should download the `README.md` file that we created on GitHub in the previous step. Look in RStudio's file browser pane for the `README.md` file.

There's a big advantage to the "GitHub first, then RStudio" workflow: the remote GitHub repo is added as a remote for your local repo and your local `master` branch is now tracking `master` on GitHub. This is a technical but important point about Git. The practical implication is that you are now set up to push and pull. No need to fanny around setting up Git remotes and tracking branches on the command line.

### Existing project

This is probably the safest and quicker way to link an existing RStudio project to GitHub through Rstudio. Simply create a new repo in GitHub and clone it as a new project with RStudio and then copy all the files from the old project to the new one.

## Tricks 

### Move files bigger than a certain size to .gitignore

```
find . -size +49M | cat >> .gitignore
```
