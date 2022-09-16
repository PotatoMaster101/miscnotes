# Bisect

- [Gist](#gist)
- [Setup Typical Git Repo](#setup-typical-git-repo)
    - [Create Code and Good Commit](#create-code-and-good-commit)
    - [Add More Commits](#add-more-commits)
    - [Create Bad Commit](#create-bad-commit)
- [Using `git bisect`](#using-git-bisect)
    - [Basic Usage](#basic-usage)
    - [Stopping Midway](#stopping-midway)
    - [Undo a Mistake](#undo-a-mistake)
- [Automatic Usage](#automatic-usage)

## Gist
You want to find out which commit introduced a bug. `git bisect` can make that process easier.

## Setup Typical Git Repo
### Create Code and Good Commit
I will be using rust as an example, but any other languages are fine:
```rust
pub fn add(a: i32, b: i32) -> i32 {
    a + b
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_add() {
        assert_eq!(add(1, 2), 3);
    }
}
```

The rust code has `add` function to add 2 numbers, and a unit test to make sure it works. Create the initial (good) commit:
```
$ git add -A
$ git commit -am 'Good commit'
$ git log --oneline
7707f48 Good commit
```

### Add More Commits
Now make some more changes to the code (in my case I added some comments) and add a few more commits to simulate some dev work:
```
$ git log --oneline
b86c998 Change no.4
52e752a Change no.3
acf69f9 Change no.2
6e738ad Change no.1
7707f48 Good commit
```

### Create Bad Commit
Time to introduce a bug. Change the `add` to perform subtraction instead (`a - b`), and create a commit:
```
$ git commit -am 'Bad commit'
```

Now add more commits on top of the bad commit:
```
$ git log --oneline
cfa3e12 Change no.7
0bcfd63 Change no.6
7677b0f Change no.5
537342a Bad commit
b86c998 Change no.4
52e752a Change no.3
acf69f9 Change no.2
6e738ad Change no.1
7707f48 Good commit
```

Once we run the unit test now it will fail:
```
$ cargo test
running 1 test
test tests::test_add ... FAILED
```

## Using `git bisect`
### Basic Usage
Before using `git bisect` you will need to know:
- A commit that doesn't contain the bug (the good commit), you can do this by picking a random commit in the past and test it
- A way to test whether a commit is good or bad, easiest way is just to run unit tests

Now using `git bisect` you can start finding the bug. You will need to start the bisect process using `git bisect start <bad> <good>`, and provide the hash to a good commit (in my case the first commit `7707f48`) and a bad commit (typically the latest commit, so `HEAD`):
```
$ git bisect start HEAD 7707f48
Bisecting: 3 revisions left to test after this (roughly 2 steps)
[b86c9989a85d8213a95d47bebe03594d927e7649] Change no.4
```

`git` has started the bisect and checked out the commit `b86c998`, which is the mid point between the good commit and the HEAD commit. You want to test if this commit is good or bad. In my case I will use `cargo test` to run unit tests:
```
$ cargo test
running 1 test
test tests::test_add ... ok
```

The test passed, so inform `git` that this commit is good using `git bisect good`:
```
$ git bisect good
Bisecting: 1 revision left to test after this (roughly 1 step)
[7677b0f7bc0f524558c2775bebc2d353a238d560] Change no.5
```

`git` checked out commit `7677b0f`, which is the mid point between `b86c998` and the HEAD commit. Repeat the same process:
```
$ cargo test
running 1 test
test tests::test_add ... FAILED
```

The test failed, inform `git` that this commit is bad using `git bisect bad`:
```
$ git bisect bad
Bisecting: 0 revisions left to test after this (roughly 0 steps)
[537342afd4f73591ca473f00a28ab7a356b5f155] Bad commit
```

`git` checked out the commit between `b86c998` and `7677b0f`, run the test again and inform `git`:
```
$ cargo test
running 1 test
test tests::test_add ... FAILED
$ git bisect bad
537342afd4f73591ca473f00a28ab7a356b5f155 is the first bad commit
```
`git` has successfully found the first commit where the bug was introduced (`537342a`), which is correct.

### Stopping Midway
You want to stop the bisect process:
```
$ git bisect reset
```

### Undo a Mistake
If you accidentally tagged a commit as good but it is actually bad (or vise versa), you can log the current bisect commands used, fix the mistake, then use replay to run all of the bisect commands again. First save the current bisect log to a file:
```
$ git bisect log > bisect.log
```

The log stores every bisect command used, so change the mistake in the log (a good to bad or bad to good). Then stop the current bisect and use replay to run the bisect process again:
```
$ git bisect reset
$ git bisect replay bisect.log
```

## Automatic Usage
Using `git bisect` manually is pretty fast, because `git` doesn't check every single commit, instead using a binary search approach (each bisect will half the number of commits to check). However sometimes you want to tell `git` to automatically perform the bisect without manually checking if a commit is good or bad. This can be done using `git bisect run` after starting:
```
$ git bisect start HEAD 7707f48
$ git bisect run cargo test
...
537342afd4f73591ca473f00a28ab7a356b5f155 is the first bad commit
```

Provide the command/script to run to determine whether a commit is good or bad, in my case using `cargo test`. The command/script should return `0` on a good commit and `1` on a bad commit.
