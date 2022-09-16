# Reuse Recorded Resolution

- [Gist](#gist)
- [Setup Merge Conflict](#setup-merge-conflict)
- [Enable `rerere`](#enable-rerere)
- [Cause a Merge Conflict](#cause-a-merge-conflict)
- [Resolve a Merge Conflict](#resolve-a-merge-conflict)
- [Undo Auto Resolved Conflict](#undo-auto-resolved-conflict)
- [Forget a Resolution](#forget-a-resolution)
- [Forget All Resolutions](#forget-all-resolutions)

## Gist
You want to tell `git` to remember the merge conflict resolution and auto apply it in the future when the same conflicting changes occur again.

## Setup Merge Conflict
First we need to setup a repo and introduce merge conflict.
```
$ echo 'hello world' > text.txt
$ git init
$ git add -A
$ git commit -am 'Initial commit'
$ git log --oneline
5362ce5 (HEAD -> main) Initial commit
```

Create another branch named `conflict` and add more commits:
```
$ git checkout -B conflict
$ echo 'hello world from conflict branch' > text.txt
$ git commit -am 'Changes in conflict branch'
$ git log --oneline
4b5e2db (HEAD -> conflict) Changes in conflict branch
5362ce5 (main) Initial commit
```

Back to `main` branch and modify the same file to introduce a merge conflict:
```
$ git checkout main
$ echo 'hello world from main branch' > text.txt
$ git commit -am 'Changes in main branch'
$ git log --oneline
94b4aad (HEAD -> main) Changes in main branch
5362ce5 Initial commit
```
Now `main` and `conflict` have conflicting changes.

## Enable `rerere`
To play with `rerere`, enable it first in config:
```
$ git config --global rerere.enabled true
```

## Cause a Merge Conflict
Now we merge `conflict` into `main`:
```
$ git merge conflict
Auto-merging text.txt
CONFLICT (content): Merge conflict in text.txt
Recorded preimage for 'text.txt'
Automatic merge failed; fix conflicts and then commit the result.
```
The line `Recorded preimage for 'text.txt'` means `rerere` has started to record this merge conflict.

## Resolve a Merge Conflict
Resolve the merge conflict to `hello world from both branches` and use `git rerere` to verify a few things:
```
$ git mergetool     # resolve to 'hello world from both branches'
$ git rerere status
text.txt
$ git rerere diff
--- a/text.txt
+++ b/text.txt
@@ -1,5 +1 @@
-<<<<<<<
-hello world from conflict branch
-=======
-hello world from main branch
->>>>>>>
+hello world from both branches
```

Using `git rerere status` will show the conflicting files, and `git rerere diff` will show the changes to remember for auto resolve. In the future when `git` sees a merge conflict containing `hello world from conflict branch` and `hello world from main branch` changes (regardless which "side" the changes are on), it will auto resolve it to `hello world from both branches`. We then commit the resolved conflict:
```
$ git commit -am 'Resolve conflict'
Recorded resolution for 'text.txt'.
```

The output `Recorded resolution for 'text.txt'.` indicates that the resolution has been saved. We can verify this by hard reset to the commit before the merge, and try causing the conflict again:
```
$ git reset --hard 94b4aad
$ git merge conflict
Auto-merging text.txt
CONFLICT (content): Merge conflict in text.txt
Resolved 'text.txt' using previous resolution.
Automatic merge failed; fix conflicts and then commit the result.
$ cat text.txt
hello world from both branches
```
As the output shows, `rerere` auto resolved to `hello world from both branches`. Run `commit` again after to commit the changes, the auto resolved changes won't be auto committed.

## Undo Auto Resolved Conflict
So you encountered another merge conflict in the future with the same "left" and "right" changes, and `rerere` auto resolved the conflict. However, this time you would like to manually resolve it to something else. You can undo the auto resolved changes with `checkout`:
```
$ git merge conflict
Auto-merging text.txt
CONFLICT (content): Merge conflict in text.txt
Resolved 'text.txt' using previous resolution.
Automatic merge failed; fix conflicts and then commit the result.
$ git checkout -m text.txt
Recreated 1 merge conflict
```
Using `checkout -m` on the conflicting file adds back the merge conflict markers, then you can manually resolve the conflict again. However this will not update the existing resolution in `rerere` so next time it will still resolve to old resolution.

## Forget a Resolution
You can tell `rerere` to forget an auto resolution, maybe you made a mistake and need to redo:
```
$ git rerere forget text.txt
```

Test the merge conflict again:
```
$ git reset --hard 94b4aad
$ git merge conflict
Auto-merging text.txt
CONFLICT (content): Merge conflict in text.txt
Recorded preimage for 'text.txt'
Automatic merge failed; fix conflicts and then commit the result.
```
As the output shows you will need to manually resolve the merge conflict again, and `rerere` will remember the new resolution after.

## Forget All Resolutions
You want to forget everything in the `rerere` history. This can be done by removing `rr-cache` directory in `.git`:
```
$ rm -rf .git/rr-cache
```
