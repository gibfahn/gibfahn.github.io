# Setting up a fork to contribute to a repo

If you're planning to contribute to an open-source project on Github, the first step is
often to create a fork. There are a couple of things you can do to set yourself
up for success.

## Step 1: Fork the repo

Go to the repo you want to fork (I'll use https://github.com/nodejs/node as an
example) and click `Fork`. If you're given an option, choose to fork the repo
as yourself.

## Step 2: Clone the fork

```bash
git clone git@github.com:gibfahn/node.git # Use your username to get your fork
cd node
```

**N.B.** If you haven't set up ssh keys, now is the time.

## Step 3: Change the default branch

A lot of people ask how the branches in a fork should be kept up to date with
the upstream. This is something that will catch you out, as sooner or later
you'll do a `git checkout master`, and git will check out your fork's master,
which will have dropped behind, meaning your PR will be all messed up etc. etc.

The best way to avoid this is to always delete all the branches on your fork,
and to always rename your remotes. You'll always want `fork` and `upstream` as
remotes anyway, so there's no downside.

```bash
# Set the remote name to fork so you know what it points to
git remote rename origin fork

# Create a new branch from master (name optional), you need at least one branch on your fork,
# it might as well be this one. You always need at least one branch on Github,
# so it makes sense to have one based on master.
git checkout -b oldmaster
# Push the branch to your fork and set it as the upstream (you can use -u
# instead of --set-upstream)
git push --set-upstream fork oldmaster
```

Now go to your fork's page on Github and
then Settings->Branches->Default branch (URL should be similar to
https://github.com/gibfahn/node/settings/branches). Change the default branch to
oldmaster (yes you understand the risks, it's your fork, so it shouldn't affect
others).

## Remove other branches

There are two ways to do this. If the repo you forked doesn't have too many
branches you can just delete them from the Github web interface. Go to
Fork->Code->Branches (e.g. https://github.com/gibfahn/node/branches) and click
the bin icon next to each branch except for `oldmaster`.

However, if the repo is anything like node, there are probably hundreds of
branches, so we should just write a little script.

```bash
# Create a list of all the branches on the fork remote except for oldmaster, and remove the fork/
# prefix. Pipe the output to a file called tmp in the current directory.
git branch -r | grep fork | grep -v oldmaster | sed 's:fork/::' >tmp
# Have a look at tmp with vim tmp (or your preferred editor). Make sure it
# contains a list of branches and isn't mangled or full of other things.

# For each line in ./tmp, delete the remote branch. The & means delete in the
# background, so your terminal will go a bit crazy for a few seconds.
for i in `cat tmp`; do echo Deleting $i from fork; git push fork :$i &; done
# Clean up the file called tmp you just created
rm tmp
```

This may take a while, but by the end you should have a nice clean repo. Check
by going to the Branches list on Github (e.g. https://github.com/gibfahn/node/branches).

## Clean up your local repo

You now just need to tell git to delete any references it may have to remote
branches that have now been deleted (things like `remotes/fork/master`).

```bash
git remote prune fork
```

## Clone the upstream

But wait, how do you see the upstream branches you'll be working from? Don't
worry, git's got you covered, you just add another remote.

```bash
# Add a remote called upstream (choose your own name). I recommend using https
# for upstream repos and ssh for forks, that way you'll be asked for a password
# if you ever try to push to a fork.
git remote add upstream https://github.com/nodejs/node.git

# Fetch from all remotes, when you add a remote git won't automatically fetch
# branches and tags from it.
git fetch --all
```

## Create a branch and start hacking

Now you're good to go, you can just create a branch and start hacking on your
changes. Most of the time you'll be raising PRs against `master`, but YMMV.

```bash
# Checkout upstream master
git checkout master
# Create a local branch based on the upstream/master, try to give your branch a
# a name that will mean something when you're looking at 10 and trying to
# remember what they all are.
git checkout -b my-new-feature

# Make your changes here

git add my/changed/paths
git commit # Pick a clear message, follow your projects guidelines
git push -u fork my-new-featuree
```

