# Question 1

What is git stash?

The git stash command takes your uncommitted changes (both staged and unstaged), saves them away for later use, and 
then reverts them from your working copy. For example:

```
$ git status
On branch main
Changes to be committed:

    new file:   style.css

Changes not staged for commit:

    modified:   index.html

$ git stash
Saved working directory and index state WIP on main: 5002d47 our new homepage
HEAD is now at 5002d47 our new homepage

$ git status
On branch main
nothing to commit, working tree clean
```

# Question 2

What is git cherry-pick?

`git cherry-pick` is a command in Git that allows you to apply a specific commit from one branch to another. 
It is commonly used to take a single commit (or a range of commits) from one branch and apply it to another branch, without merging the entire branch.

Usage:
```bash
git cherry-pick <commit-hash>
```

This command will apply the changes introduced by the specified commit to your current branch.
You can also cherry-pick multiple commits by specifying a range of commit hashes.

# Question 3

What is the difference between git fetch and git pull?

The main difference between `git fetch` and `git pull` is how they update the local repository with changes from the remote repository:

1. `git fetch`: This command retrieves new commits from the remote repository but does not integrate them into the current working branch.
    It simply downloads the changes to your local repository, allowing you to inspect them before merging.

2. `git pull`: This command fetches new commits from the remote repository and integrates them into the current working branch. I
   t automatically performs a `git fetch` followed by a `git merge` or `git rebase` to update your local branch with the changes from the remote.

In summary, `git fetch` downloads changes from the remote repository without merging them into your working branch, 
while `git pull` downloads and merges changes from the remote repository into your working branch in one step.
