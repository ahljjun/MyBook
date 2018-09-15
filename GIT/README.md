

Git Squash:

You can use`git merge --squash`for this, which is slightly more elegant than`git rebase -i`. Suppose you're on master and you want to squash the last 12 commits into one.

WARNING: First make sure you commit your work—check that`git status`is clean \(since`git reset --hard`will throw away staged and unstaged changes\)

Then:

```
# Reset the current branch to the commit just before the last 12:
git reset --hard HEAD~12

# HEAD@{1} is where the branch was just before the previous command.
# This command sets the state of the index to be as it would just
# after a merge from that commit:
git merge --squash HEAD@{1}

# Commit those squashed changes.  The commit message will be helpfully
# prepopulated with the commit messages of all the squashed commits:
git commit

```

The [documentation for`git merge`](http://www.kernel.org/pub/software/scm/git/docs/git-merge.html)describes the`--squash`option in more detail.

the only real advantage of this method over the simpler

`git reset --soft HEAD~12 && git commit`

suggested by Chris Johnsen in [his answer](https://stackoverflow.com/questions/5189560/how-can-i-squash-my-last-x-commits-together-using-git/5201642#5201642) is that you get the commit message prepopulated with every commit message that you're squashing.

