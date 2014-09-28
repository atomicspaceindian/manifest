# VentureROM #

## Background ##

[Repo](http://source.android.com/source/developing.html) is a tool provided by Google that
simplifies using [Git](https://www.atlassian.com/git/tutorials) in the context of the Android source.

## Setting up the Build Environment ##
Many different Linux distributions exist, and here we will outline a few of the popular ones
The suggested [guide] (http://forum.xda-developers.com/showthread.php?t=2464683) for [Ubuntu] (http://www.ubuntu.com/) works on versions 13.10 and above.

Our guide for [Arch] (https://www.archlinux.org/) is in development.

### Installing Repo ###

```bash
# Make a directory where Repo will be stored and add it to the path
$ mkdir ~/bin
$ PATH=~/bin:$PATH

# Download Repo itself
$ curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo

# Make Repo executable
$ chmod a+x ~/bin/repo
```

### Initializing Repo ###

```bash
# Create a directory for the source files
# This can be located anywhere (as long as the fs is case-sensitive)
$ mkdir android
$ cd android

# Install Repo in the created directory
# Use a real name/email combination, if you intend to submit patches
$ repo init -u https://github.com/VentureROM/manifest -b kitkat
```

You may be prompted to enter some ID if youa re running this for the first time.
```bash
# Give your ID so we can identify if you try to help us with code
$ git config --global user.email "<enter email>"
$ git config --global user.name "<enter your name>"
```
Then re-run the repo initialization.

### Downloading the source tree ###

This is what you will run each time you want to pull in upstream changes. Keep in mind that on your
first run, it is expected to take a while as it will download all the required Android source files
and their change histories. Afterwatrds, only changed files.

```bash
# Let Repo take care of all the hard work
$ repo sync
```

#### Syncing specific projects ####

In case you are not interested in syncing all the projects, you can specify what projects you do
want to sync. This can help if, for example, you want to make a quick change and quickly push it
back for review. You should note that this can sometimes cause issues when building if there is
a large change that spans across multiple projects.

```bash
# Specify one or more projects by either name or path

# For example, enter VentureROM/android_frameworks_base or
# frameworks/base to sync the frameworks/base repository

$ repo sync PROJECT
```

#### Using CCACHE ####
CCACHE speeds up builds by files from the previous build. It is recommended to clear CCACHE when building for a public release
```bash
# Lets initialize it first

# Go to the directory in which you are building
$ cd ~/android

# Lets give the command for CCACHE
$ prebuilts/misc/linux-x86/ccache/ccache -M 50G
# This will allot 50GB to CCACHE. change the '50' to however much you'd want to give. At least 25 is recommended.

# Now lets tell our computer where CCACHE is so it can use it

# CD into your Home folder
$ cd ~/

# Then lets tell it
$ sudo nano .bashrc

# Add this line to the very end
export USE_CCACHE=1
# It should right after "export PATH=~/bin:$PATH" which you added earlier

# Now lets refresh bashrc. You can do this by logging out and logging back in, but who likes to do that?
$ source .bashrc
```

## Building ##

The bundled builder tool `./rom-build.sh` handles all the building steps for the specified device
automatically. As the device value, you just feed it with the device codename (for example,
'hammerhead' for the Nexus 5).

```bash
# Go to the root of the source tree, in our case "android"...
$ cd android
# ...and run the builder tool.
$ ./rom-build.sh <device>
```


## Helping us with changes ##

```bash
# Start by going to the repo on Github and forking it to your own profile

# Clone that project to your machine
$ git clone git://www.github.com/<URL of the forked project>.git

# Make your changes
...

# Commit all your changes
$ git add -A
$ git commit -a

# Push your changes to your Github account
$ git push

# Send it to us for review
# Go to the Pull Requests tab on your forked repo and create a new pull request
# Give us a description of it, leave a comment or two perhaps.
```
If your code is good and benefitial, we can gladly consider including it!

### Writing good commit messages ###

You will be asked a commit message when you run `git commit`. Writing a good commit message is
often hard, but it is also essential as these messages will stay around with your changes and
will be seen by others when looking back at the project history.

A few general pointers to keep in mind when writing the commit message are that you should use
imperative as it matches the style used by the `git merge` and `git revert` commands (that means
"Fix bug" is preferred over "Fixes bug", "Fixed bug" and others) and that you should write the
first line of the commit message as a summary of the commit. It should always be capitalized and
followed by an empty line. You might optionally include the project name at the start and try to
keep it to 50 characters when possible as it is used in various logs, including "one line" logs.
