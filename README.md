# 1. Workflow
## 1-1. Edit files
Choose appropriate files for the branch and edit them.
Do not make changes longer and avoid increasing 
the number of changed files as much as possible.
We would rather recommend to create a new branch.

### 1-1-A. When we would like to work on new features in the branch that has not been merged yet

1. Checkout the branch and create a new branch
Follow the project naming rules.
```
$ git checkout <target branch name>
$ git checkout -b <new branch name>
```

2. Work on the new branch and send a PR
If the target branch is updated, update this branch.
```
# Forked repostiory
git pull --rebase origin <target branch name>
# Otherwise
git pull --rebase upstream <target branch name>
```

If the target branch is merged, make the base branch `<remote branch name>`, otherwise, keep the base branch the target branch.

## 1-2. Check the changes in your branch
```
$ git status
```

## 1-3. Add files on the staging environment
It would be better to add files one by one
rather than using the -all options.
```
$ git add <file>
```

## 1-4. Local commit
This commit message should follow the project rule.

```
$ git commit -m “<commit message>”
```

## 1-5. Check the update on the remote repository and apply the changes to the local repository
Before pushing, fetch the latest version. 
In the case of working on a **forked repository**, fetch from upstream.

```
# Forked repository
$ git pull --rebase upstream <remote branch name>
# Otherwise 
$ git pull --rebase origin <remote branch name>
```

### 1-5-A. If there are conflicts
Fix the files properly. 
These changes lead to the changes of others’ commits, 
so it would be better to discuss the changes in advance.

After the changes, add files one by one.
```
$ git add <fixed file>
```

Complete rebase.
```
$ git rebase --continue
```

## 1-6. Upload changes to the remote repository
```
$ git push origin <local branch name>
```

## 1-7. Send a pull request
Send a pull request from the github website.
Note: Push the green button saying **New pull request**. 

## 1-8. Wait for reviews and fix the issues mentioned by the reviewers
Repeat the steps 3. ~ 7. until the PR is merged to the remote repository.

## 1-9. Update the local environment
```
# Forked repository
$ git pull --rebase upstream <remote branch name>
# Otherwise
$ git pull --rebase origin <remote branch name>

# When there are conflicts, do the followings after editing
git add <conflicted file>
git commit -m "Resolved merge conflict by incorporating incoming changes"
```

## 1-10. Remove the branch if you do not need it
```
git branch --delete <local branch name>
git push --delete origin <deleted branch name>
```

# 2. Github conventions
## 2-1. Branching rules
First, the main branches for each repository should have `main` and `stable` branches.
`main` branch is a working branch and `stable` branch is a branch that provides stable functionalities as intended.

Branch name should look like the following:
```
# <Issue Tracker ID>-<work type>-<work description>
123-hotfix-add-foo-function
```
There are four rules for the naming.
1. The name always starts from the issue ID in the Github webpage
2. Work type is listed in the table below
3. Use a short and recognizable name
4. The delimiter must be hyphens 

|work type|description|
|-|-|
|hotfix|branch from `stable` and urgent work|
|feature|branch from `main` and not urgent work|
|bug|branch from `main` and urgent work such as bugs|

[References]
1. [Github branching standards and conventions](https://gist.github.com/digitaljhelms/4287848)
2. [Git branch naming conventions](https://deepsource.io/blog/git-branch-naming-conventions/)

## 2-2. Commit messages
1. Start with a tag
2. Separate subject from body with a blank line
3. Limit the subject line to 50 characters
4. Capitalize the first word of the subject line
5. Do not end the subject line with a period
6. Use the imperative mood in the subject line (not -ing or -ed) 
7. Make new lines every 72 characters in the body
8. Use the body to explain what and why vs how

Create the file `~/.gitmessage` and paste the template below:
```
# [<tag>] (If applied, this commit will...) <subject> (Max 72 char)
# |<----   Preferably using up to 50 chars   --->|<------------------->|
# Example:
# [feat] Implement automated commit messages

# (Optional) Explain why this change is being made
# |<----   Try To Limit Each Line to a Maximum Of 72 Characters   ---->|

# (Optional) Provide links or keys to any relevant tickets, articles or other resources
# Example: Github issue #23

# --- COMMIT END ---
# Tag can be 
#    feat     (new feature)
#    fix      (bug fix)
#    refactor (refactoring code)
#    style    (formatting, missing semi colons, etc; no code change)
#    doc      (changes to documentation)
#    test     (adding or refactoring tests; no production code change)
#    version  (version bump/new release; no production code change)
#    jsrXXX   (Patches related to the implementation of jsrXXX, where XXX the JSR number)
#    jdkX     (Patches related to supporting jdkX as the host VM, where X the JDK version)
#    dbg      (Changes in debugging code/frameworks; no production code change)
#    license  (Edits regarding licensing; no production code change)
#    hack     (Temporary fix to make things move forward; please avoid it)
#    WIP      (Work In Progress; for intermediate commits to keep patches reasonably sized)
#    defaults (changes default options)
#
# Note: Multiple tags can be combined, e.g. [fix][jsr292] Fix issue X with methodhandles
# --------------------
# Remember to:
#   * Capitalize the subject line
#   * Use the imperative mood in the subject line
#   * Do not end the subject line with a period
#   * Separate subject from body with a blank line
#   * Use the body to explain what and why vs. how
#   * Can use multiple lines with "-" or "*" for bullet points in body
# --------------------
```

To tell Git to use the template file (globally, not just in the current repo), 
use the following command:
```
git config --global commit.template ~/.gitmessage
```

[References]
1. [Using Git commit message templates to write better commit messages](https://gist.github.com/lisawolderiksen/a7b99d94c92c6671181611be1641c733)
2. [How to write a git commit message](https://chris.beams.io/posts/git-commit/)
3. [One example of git commit message template ](https://gist.github.com/zakkak/7e06725ebd1336bfebebe254de3de825)
4. [A practical guide to small and easy-to-review pull requests](https://sourcediving.com/a-practical-guide-to-small-and-easy-to-review-pull-requests-a7f04a01d5d5)

# 3. Git useful command
1. cancel the stating of the file
```
git reset HEAD <file name>
```

2. Check the commit history (option: -p, --stat, --graph, --oneline, -<number>)
```
git log
or
git log origin/<branch name>
```

3. Check the differences between branches, files and so on (option: --cached, --name-only, --stat, --word-diff)
```
git diff <target1> <target2>
```

4. Check who changed the file (option: -e, -f, -n)
```
4. git blame <file name>
```
Useful when you try to contact the person for the discussion or the intention behind the code

5. git rebase usage
Cancel the rebase
```
$ git rebase --abort
```

Continue the rebase (Use it when conflicts happen)
```
$ git rebase --continue
```

Group a bunch of commits (We can check ID by git log.)
```
$ git rebase -i <group commits after this ID>
```

6. Escape the current changes
```
# escape the current changes
$ git stash
# Recover the escaped changes
$ git stash apply
# Clear the escaped changes  (Check if the changes are really recovered before using this command.)
$ git stash clear
```

7. Check the base branch of the current branch
```
$ git show-branch | grep '*' | grep -v "$(git rev-parse --abbrev-ref HEAD)" | head -1 | awk -F'[]~^[]' '{print $2}'
```

8. Find files that have a specific sentence
```
# We can use wild card "*" for file name 
find -name “<file name>” -exec grep "<target sentence>" {} \; -print
```
