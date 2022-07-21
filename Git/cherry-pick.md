# Cherry-pick

- [Gist](#gist)
- [Basic Usage](#basic-usage)
- [No Commit](#no-commit)
- [Pick a Range](#pick-a-range)

## Gist
Use `cherry-pick` to apply another commit from another branch into the current branch.

## Basic Usage
Initialize repo with a few branches simulating different developers:
```
# main branch
$ git init
$ echo 'main' > main.txt
$ git add -A
$ git commit -am 'Add main.txt'         # hash = 6106ccb

# first dev's branch based off main
$ git checkout -B branch2 main
$ echo 'branch2' > branch2.txt
$ git add -A
$ git commit -am 'Add branch2.txt'      # hash = c5988c7

# second dev's branch based off main
$ git checkout -B branch3 main
$ echo 'branch3' > branch3.txt
$ git add -A
$ git commit -am 'Add branch3.txt'      # hash = d1383ca
```

For some reason first dev wants second dev's `branch3.txt`:
```
$ git checkout branch2
$ git log --oneline
c5988c7 (HEAD -> branch2) Add branch2.txt
6106ccb (main) Add main.txt
$ git cherry-pick d1383ca
$ git log --oneline
918c814 (HEAD -> branch2) Add branch3.txt
c5988c7 Add branch2.txt
6106ccb (main) Add main.txt
```
`cherry-pick` will copy that commit into the current branch. A ending hash will be assigned to the new commit.

## No Commit
Sometimes we don't want a commit after using `cherry-pick`, for example if we want to change something after `cherry-pick` then commit ourselves. This can be done using `-n`:
```
$ git checkout branch2
$ git log --oneline
c5988c7 (HEAD -> branch2) Add branch2.txt
6106ccb (main) Add main.txt
$ git cherry-pick d1383ca -n
$ git status
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   branch3.txt
$ git commit -am 'Add new file'
$ git log --oneline
e0079f6 (HEAD -> branch2) Add new file
c5988c7 Add branch2.txt
6106ccb (main) Add main.txt
```
`cherry-pick` will apply the changes in that commit into the current branch.

## Pick a Range
Sometimes we want to pick multiple consecutive commits. For this use the range syntax:
```
$ git cherry-pick <starting hash>..<ending hash>
```

`<starting hash>` is the "start point" and `ending hash` is the "end point" of the range. Note the `<starting hash>` commit will not be included in the `cherry-pick`, to include it use `^` after the hash:
```
$ git cherry-pick <starting hash>^..<ending hash>
```
