# 1. Workflow

You can also check [General guideline](https://github.com/elsewhencode/project-guidelines).

## 1-0. Register upstream
When cloning a new project from not your repository,
you need to define the reference name for the remote repository.
(Plus, do not forget to push `Fork` button in the GitHub GUI.)
Conventionally, we name it `upstream`.

```
$ git remote add upstream <URL of the upstream repository>
```
Additionally, you have to add your repository as the origin.

```
$ git remote add origin <URL of your repository>
```
If you made a mistake when you add `origin`,
you can re-do it as follows:

```
$ git remote rm origin
$ git remote add origin <URL of your repository>
```

## 1-1. Edit files
Choose appropriate files for the branch and edit them.
Do not make changes longer and avoid increasing 
the number of changed files as much as possible.
We would rather recommend to create a new branch.

[References]
1. [How to Split Pull Requests – Good Practices, Methods and Git Strategies / Git strategy 3. Sequential PRs of “milestone” commits](https://www.thedroidsonroids.com/blog/splitting-pull-request#git-strategy-3)
1. [When should I create a new branch? Stack overflow](https://stackoverflow.com/questions/15633409/when-should-i-create-a-new-branch)

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
$ git pull --rebase origin <target branch name>
# Otherwise
$ git pull --rebase upstream <target branch name>
```
**NOTE**: Since it can overwrite your local change,
do not forget to take a backup for your changes.

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
# If the changes are small
$ git commit -m “<commit message>”
# Otherwise, add the body
$ git commit
```
Note that commit message with the body works when you setup the git commit template described in 2.2.

## 1-5. Check the update on the remote repository and apply the changes to the local repository
Before pushing, fetch the latest version. 
In the case of working on a **forked repository**, fetch from upstream.

```
# Forked repository
$ git pull --rebase upstream <remote branch name>
# Otherwise 
$ git pull --rebase origin <remote branch name>
```
**NOTE**: Since it can overwrite your local change,
do not forget to take a backup for your changes.

### 1-5-A. If there are conflicts
Fix the files properly. 
These changes lead to the changes of others’ commits, 
so it would be better to discuss the changes in advance.
Note that `incoming change` is the change that you made
and `current change` is the change that
you get from the remote repository.

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
Send a pull request from the GitHub website.
When sending a PR, follow the rules specified in 2-3-A.
**NOTE**: Push the green button saying **New pull request**.

### 1-7-A. When you get a lot of old commits in PR
We often encounter the situation where we get a lot of old commits in PR.
In this case, we have to apply the following:
```
$ git checkout <local branch name>

# Forked repository
$ git pull --rebase upstream <remote branch name>
# Otherwise
$ git pull --rebase origin <remote branch name>

$ git push origin <local branch name>
``` 

## 1-8. Wait for reviews and fix the issues mentioned by the reviewers
Repeat step 1.3. ~ 1.7. until the PR is merged to the remote repository.
When we merge PR, we would use `Squash and merge`
instead of `Create a merge commit` from the Github webpage.
If you are a reviewer, it is helpful to use the comparison feature in Github:
```
https://github.com/<Repository owner name>/<Repository name>/compare/<Commit hash 1>...<Commit hash 2>
```

## 1-9. Update the local environment
```
# Forked repository
$ git pull --rebase upstream <remote branch name>
# Otherwise
$ git pull --rebase origin <remote branch name>

# When there are conflicts, do the following after editing
$ git add <conflicted file>
$ git commit -m "[fix] Resolve merge conflict by incorporating incoming changes"
```

When you would like to rebase after the `Squash and merge`,
you may need to handle `rebase --continue` many times
and it is time-consuming.
One way to avoid this is the following:
```
$ git checkout <current branch name>
# Take the log for the post-processing
$ git log

$ git checkout <branch to rebase>
$ git branch -m <current branch name> <another name>
$ git branch -D <another name>
$ git checkout -b <current branch name>

$ git cherry-pick <commit hash to put on the top>
```

## 1-10. Remove the branch if you do not need it
```
$ git branch --delete <local branch name>
$ git push --delete origin <deleted branch name>
```

# 2. Github conventions
## 2-1. Branching rules
First, the main branches for each repository should have `main` and `stable` branches.
`main` branch is a working branch and `stable` branch is a branch that provides stable functionalities as intended.

The branch name should look like the following:
```
# <Issue Tracker ID>-<work type>-<work description>
123-feature-add-foo-function
```
There are four rules for naming.
1. The name always starts from the issue ID in the Github webpage
2. Work type is listed in the table below
3. Use a short and recognizable name
4. The delimiter must be hyphens 

|work type|description|
|-|-|
|hotfix|branch from `stable` and urgent work|
|feat|branch from `main` and work on new feature|
|fix|branch from `main` and fix some issues|
|refactor|branch from `main` and work on refactoring|

[References]
1. [Github branching standards and conventions](https://gist.github.com/digitaljhelms/4287848)
2. [Git branch naming conventions](https://deepsource.io/blog/git-branch-naming-conventions/)

## 2-2. Commit messages
The details are mentioned in the reference below.
Basically, the commit message should specify why you make the changes
because people will know what you changed from `git diff` and cannot know why from the git commands.
There are seven rules to follow:
1. Start with a tag
2. Separate subject from body with a blank line
3. Limit the subject line to 50 characters
4. Capitalize the first word of the subject line
5. Do not end the subject line with a period
6. Use the imperative mood in the subject line (not -ing or -ed) 
7. Make new lines every 72 characters in the body
8. Use the body to explain what and why vs how

Create the file `~/.github/.gitmessage` and paste the template below:
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
Note that `#` is comment-out, so we do not have to delete everything.

To tell Git to use the template file (globally, not just in the current repo), 
use the following command:
```
$ git config --global commit.template ~/.github/.gitmessage
```

To customize the editor (e.g. emacs, vim) for commit message, use the following command:
```
$ git config --global core.editor <editor name>
```

[References]
1. [Using Git commit message templates to write better commit messages](https://gist.github.com/lisawolderiksen/a7b99d94c92c6671181611be1641c733)
2. [How to write a git commit message](https://chris.beams.io/posts/git-commit/)
3. [One example of git commit message template ](https://gist.github.com/zakkak/7e06725ebd1336bfebebe254de3de825)
4. [8.1 Customizing Git](https://git-scm.com/book/en/v2/Customizing-Git-Git-Configuration)

## 2-3. Pull request and issue
### 2-3-A. Rules
These rules are helpful for both contributors and reviewers,
because they make the PR procedure smoother and quicker.
We recommend to look at your codes once more to check if they are already easier to review.
1. Make it small (split PR within a small number of files)
2. Do only one thing
3. Add documentation string ([Google python docstrings](https://sphinxcontrib-napoleon.readthedocs.io/en/latest/example_google.html)) and code explanations
4. Add tests
5. Push as soon as possible and do not keep changes only in your local repository
6. Separate refactoring from features

Note that we prefer code explanations in the actual codes rather than PR comments,
because they should not be used only for temporal explanations.
However, it is also promoted to put comments on the Github webpage for the PR to dispel reviewers' doubt beforehand.

For the optional strategies, it is recommended to follow:
1. Make PRs for each small change when you would like to change small issues across many files such as changing variable names
2. Units first, integration later (new PRs for each unit)

[References]
1. [10 tips for better Pull Requests](https://blog.ploeh.dk/2015/01/15/10-tips-for-better-pull-requests/)
2. [The (written) unwritten guide to pull requests](https://www.atlassian.com/blog/git/written-unwritten-guide-pull-requests)
3. [PULL REQUEST GUIDELINES](https://opensource.creativecommons.org/contributing-code/pr-guidelines/)
4. [Pull Request Etiquette - A set of simple rules for your code review](https://erikzaadi.com/2019/09/29/pull-request-etiquette-a-set-of-simple-rules-for-your-code-review/)
5. [The Art of Small Pull Requests](https://essenceofcode.com/2019/10/29/the-art-of-small-pull-requests/)
6. [How to Split Pull Requests – Good Practices, Methods and Git Strategies](https://www.thedroidsonroids.com/blog/splitting-pull-request)


### 2-3-B. Message
To recall what to write in PRs and issues, put `pull_request_template.md`
and `issue_template.md` in the home directory.
The example templates are available in the references.

[References]
1. [A Github pull request template for your projects](https://embeddedartistry.com/blog/2017/08/04/a-github-pull-request-template-for-your-projects/)
2. [Github templates: The smarter way to formalize pull requests among development teams](https://betterprogramming.pub/github-templates-the-smarter-way-to-formalize-pull-requests-among-development-teams-89f8d6a204f)
3. [10 examples of issue templates](https://github.com/stevemao/github-issue-templates)
4. [A practical guide to small and easy-to-review pull requests](https://sourcediving.com/a-practical-guide-to-small-and-easy-to-review-pull-requests-a7f04a01d5d5)

# 3. Git useful commands
1. cancel the staging of the file
```
$ git reset HEAD <file name>
```

2. Check the commit history (option: -p, --stat, --graph, --oneline, -<number>)
```
$ git log
or
$ git log origin/<branch name>
```

3. Check the differences between branches, files and so on (option: --cached, --name-only, --stat, --word-diff)
```
$ git diff <target1> <target2>
```

4. Check who changed the file (option: -e, -f, -n)
```
$ git blame <file name>
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
$ find -name “<file name>” -exec grep "<target sentence>" {} \; -print
```
