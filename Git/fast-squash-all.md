# Squash All Commits Fast

- [Gist](#gist)
- [Using `git rebase` (Hard Way)](#using-git-rebase-hard-way)
- [Using `git reset` (Easy Way)](#using-git-reset-easy-way)
- [Using `git merge` (Easiest Way)](#using-git-merge-easiest-way)

## Gist
Ever had an old messy branch with lots of commits and merge conflicts and you want to squash all commits into one single commit? Perhaps your pull request workflow is to perform a "squash merge" so `main` branch will be clean.

## Using `git rebase` (Hard Way)
Your first instinct would be to use `git rebase -i` and change the `pick` to `squash`. This works most of the cases on short lived branches, however for an old messy branch with lots of merge conflicts resolved this solution might cause some problems:
```
$ git rebase -i HEAD~50
CONFLICT (content): Merge conflict in ...
```
Oh no! Now you must use `git mergetool` and fix them before you can squash the commits.

## Using `git reset` (Easy Way)
Using `git reset --soft` you can "undo" commits. The commits will disappear and the changes done in the commits will be back in the stage, which then can be committed again. Using this we can bring all of the changes in a branch to the stage, then commit again to produce a single squashed commit.

For example, we want to squash everything in `messy-branch` into a single commit:
```
$ git checkout -b new-branch messy-branch
$ git reset --soft main
$ git commit -m 'All squashed'
```

We created a `new-branch` based on `messy-branch`, then we soft reset to `main` which will bring all changes in `messy-branch` into the stage. Now a single commit will finish the job. If `messy-branch` is already on remote, there will be more work:
```
$ git branch -D messy-branch
$ git branch -m new-branch messy-branch
$ git push -f
```
Replace the old `messy-branch` with `new-branch`, and force push the commit onto remote.

## Using `git merge` (Easiest Way)
Using `git merge --squash` you can achieve the same outcome as `git reset --soft`, it will bring in all the changes from a branch into the stage which then you can commit to a single commit:
```
$ git checkout -b new-branch origin/main
$ git merge --squash messy-branch
$ git commit -m 'All squashed'
```
