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

# Question 4

Difference between git merge and git rebase.

In summary, while both git merge and git rebase are used to integrate changes between branches, they have different effects on the commit history. Merge creates a new merge commit, preserving the history of both branches, while rebase rewrites the commit history by replaying commits onto a different branch, resulting in a linear history. The choice between merge and rebase depends on your workflow, preferences, and the requirements of your project.

# Question 5

What is the purpose of .gitignore file?

The **.gitignore** file is a text file used by Git to specify intentionally untracked files and directories that Git should ignore. It allows developers to exclude certain files and directories from being tracked by Git, preventing them from being included in commits and pushed to the remote repository. The main purpose of the .gitignore file is to help maintain a clean and organized repository by filtering out files and directories that are not relevant to the project or that should not be version-controlled.

# Question 6

Describe usage of .gitattributes file?

The **.gitattributes** file in Git is used to specify attributes for files and directories in a repository. It allows you to define how Git should treat certain files, such as how they should be handled during merges, line endings, and diffs. The main purpose of the .gitattributes file is to customize the behavior of Git for specific files or file patterns within a project. 

# Question 7

How do we undo the last commit?

To undo the last Git commit, you can use the `git reset` or `git revert` command depending on whether you want to keep the changes made in the commit or discard them completely. Here's how to undo the last Git commit using each method:

1. **Using `git reset` (Discarding Changes)**:
   If you want to completely undo the last commit and discard any changes made in that commit, you can use `git reset --hard HEAD~1`. This command will move the `HEAD` pointer to the previous commit, effectively removing the last commit from the commit history.

   ```bash
   git reset --hard HEAD~1
   ```

   - `git reset`: Resets the current branch to the specified commit.
   - `--hard`: Resets both the working directory and the index to match the specified commit.
   - `HEAD~1`: Specifies the commit to which you want to reset. `HEAD~1` refers to the commit immediately before the current `HEAD`.

   **Caution**: This command will discard any changes made in the last commit, and these changes will be lost permanently.

2. **Using `git revert` (Preserving Changes)**:
   If you want to undo the last commit but preserve the changes made in that commit, you can use `git revert HEAD`. This command will create a new commit that undoes the changes introduced by the last commit, effectively reverting the commit.

   ```bash
   git revert HEAD
   ```

   - `git revert`: Creates a new commit that undoes the changes introduced by a previous commit.
   - `HEAD`: Specifies the commit to be reverted, which is the last commit in this case.

   Unlike `git reset --hard`, `git revert` creates a new commit that undoes the changes from the specified commit, leaving the commit history intact. This method is safer if you want to preserve the commit history and collaborate with other developers who may have already pulled the changes.

Choose the appropriate method based on whether you want to preserve or discard the changes made in the last commit. Make sure to use these commands with caution, especially if you have already pushed the commits to a remote repository.
