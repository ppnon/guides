# GIT

SCM: Software Configuration Management

1. [Clone repository](#clone-repository)
2. [Branches](#branches)
3. [Tags](#tags)
4. [Commands](#commands)
5. [Git Flow](#git-flow)
    1. [Develop branch (develop)](#develop-branch-develop)
    2. [Feature branch (feature/my-feature)](#feature-branch-featuremy-feature)
    3. [Feature branch with Pull Request (feature/my-feature)](#feature-branch-with-pull-request-featuremy-feature)
    4. [Release branch (release/1.0.0)](#release-branch-release100)
    5. [Hotfix branch (hotfix/1.0.1)](#hotfix-branch-hotfix101)
6. [Common errors](#common-errors)
7. [References](#references)


----
## Clone repository

```bash
git clone ${your_repository} ${local_directory}

# check local and remote branches
git branch -a
```
## Connect to repository & branch
```bash
git remote add origin ${repository}
git checkout -b ${local} ${remote}

# with git-flow
git flow ${branch} track ${name}
```
[back to top](#git)

## Branches
##### Create
```bash
# create local branch
git branch ${new_branch} ${source_branch}

# create remote branch from local branch
git push --set-upstream origin ${new_branch}

# list all branches
git branch -a

# clone remote branch
git checkout ${branch}

# clone and create local branch
git checkout -b ${new_branch} ${source_branch}

# update local branch from remote
git pull ${branch}
```
##### Delete
```bash
# delete remote branch
git push origin --delete ${branch}

# delete local branch
git branch -d ${branch}

# if the local branch is connected to a remote branch, do this before deleting the local branch
git fetch -p
```
[back to top](#git)

## Tags
##### Create
```bash
# create local tag
git tag 1.0.0
git tag -a 1.0.0 ${branch} -m 'message'

# push local tags to remote
git push origin --tags

# see tag data
git show ${tag}

# list all tags
git tag -l
```
##### Delete
```bash
# delete remote tag
git push origin --delete ${tag}

# delete local tag
git tag -d ${tag}
```
[back to top](#git)

## Commands
```bash
# fetch branch from remote
git fetch
git fetch origin
git fetch ${local}:${remote}

# fetch and merge branch from remote
git pull
git pull origin
git pull ${local} ${remote}

# add files
git add ${file}
git add .

# remove added files
git rm --cached ${file}
git rm --cached -r ${directory}

# commit changes
git commit -ma "${message}"
git commit --amend # edit previous message

# status of local repository
git status

# check differences
git diff
git diff --color-words
git diff --stat
git diff --check
git diff ${branch} ${branch}

# backup uncommitted changes
git stash -u
git stash show
git stash list
git stash pop

# check logs
git logs
git logs --oneline
git show

# restore / revert / reset / clean
git restore .
git revert ${commit}
git reset --hard origin
git clean -f
```
[back to top](#git)

## Git Flow
### Develop branch (develop)

1. #### Git
    ```bash
    # Create local branch develop
    # Method A
    git branch develop master
    git switch develop

    # Method B
    git checkout -b develop master
    
    # Push local branch to Repsitory
    git push --set-upstream origin develop
    ```

2. #### Git Flow
    ```bash
    # Create local branch develop
    git flow init

    # Push local branch to Repsitory
    git push --set-upstream origin develop
    ```
[back to top](#git)

### Feature branch (feature/my-feature)

> ### develop >> feature >> develop

1. Git
    ```bash
    # Start feature
    git checkout -b feature/my-feature develop
    git push --set-upstream origin feature/my-feature
    
    # Finish feature (after commiting all your stuffs)
    git push origin feature/my-feature
    
    git checkout develop
    git merge --no-ff feature/my-feature
    git push origin develop

    # Note: is a common practice to delete the feature branch at the end
    ```

2. Git Flow
    ```bash
    # Start feature
    git flow init
    git flow feature start my-feature
    git flow feature publish my-feature

    # Finish feature (after commiting all your stuffs)
    git push origin feature/my-feature

    # Method A:
    git flow feature finish my-feature --no-ff
    git push origin develop

    # Method B:
    git flow feature finish my-feature --no-ff --push

    # Note: git-flow by default delete the feature branch if you want to keep it add -k
    ```
[back to top](#git)

## Feature branch with Pull Request (feature/my-feature)

> ### develop >> feature >> develop

1. Git
    ```bash
    # Start feature
    git checkout -b feature/my-feature develop
    git push --set-upstream origin feature/my-feature
    
    # Finish feature (after commiting all your stuffs)
    git push origin feature/my-feature
    
    git checkout develop
    git merge --no-ff feature/my-feature
    # 1.To merge your local branch with the remote branch go online and request a pull request
    # 2.After the pull request is approved you must merge your local develop branch with the remote branch
    git checkout develop
    git pull develop

    # Note: is a common practice to delete the feature branch at the end
    ```
2. Git Flow
    ```bash
    # Start feature
    git flow init
    git flow feature start my-feature
    git flow feature publish my-feature

    # Finish feature (after commiting all your stuffs)
    git push origin feature/my-feature

    git flow feature finish my-feature --no-ff -k
    # 1.To merge your local branch with the remote branch go online and request a pull request
    # 2.After the pull request is approved you must merge your local develop branch with the remote branch
    git checkout develop
    git pull develop

    # Note: git-flow by default delete the feature branch if you want to keep it add -k
    ```
[back to top](#git)

## Release branch (release/1.0.0)

> ### develop >> release >> master & develop

1. Git
    ```bash
    # Start release
    git checkout -b release/1.0.0 develop
    git push --set-upstream origin release/1.0.0

    # Finish release (after commiting all your stuffs)
    git push origin release/1.0.0
    
    git checkout master
    git merge --no-ff release/1.0.0

    git tag -a 1.0.0

    git checkout develop
    git merge --no-ff 1.0.0

    git push origin develop
    git push origin master
    git push origin --tags

    git branch -d release/1.0.0
    git push origin --delete release/1.0.0
    # Note: only one release can exist at the same time, if you have several releases in production, you must use a support branch
    ```

2. Git Flow
    ```bash
    # Start release
    git flow init
    git flow release start 1.0.0
    git flow release publish 1.0.0

    # Finish release (after commiting all your stuffs)
    git push origin release/1.0.0

    # Method A:
    git flow release finish 1.0.0

    git push origin develop
    git push origin master
    git push origin --tags

    # Note: if you are only working on one release at a time, you can use this command to push all: 
    # git push origin --all --follow-tags

    # Method B:
    git flow release finish 1.0.0 --push --pushtag 

    # Note: git-flow by default delete the release branch if you want to keep it add -k
    ```
[back to top](#git)

## Hotfix branch (hotfix/1.0.1)

> ### master >> hotfix >> master & develop

1. Git
    ```bash
    # Start hotfix
    git checkout -b hotfix/1.0.1 master
    git push --set-upstream origin hotfix/1.0.1

    # Finish hotfix (after commiting all your stuffs)
    git push origin hotfix/1.0.1
    
    git checkout master
    git merge --no-ff hotfix/1.0.1

    git tag -a 1.0.1

    git checkout develop
    git merge --no-ff 1.0.1

    git push origin master
    git push origin develop
    git push origin --tags

    # Note: is a common practice to delete the hotfix branch at the end
    ```

2. Git Flow
    ```bash
    # Start hotfix
    git flow init
    git flow hotfix start 1.0.1
    git flow hotfix publish 1.0.1

    # Finish hotfix (after commiting all your stuffs)
    git push origin hotfix/1.0.1

    # Method A:
    git flow hotfix finish 1.0.1

    git push origin master
    git push origin develop
    #git push origin --tags

    # Note: if you are only working on one hotfix at a time, you can use this command to push all: 
    # git push origin --all --follow-tags

    # Method B:
    git flow hotfix finish 1.0.0 --push

    # Note: git-flow by default delete the hotfix branch if you want to keep it add -k
    ```
[back to top](#git)

## Common errors
```bash
# refusing to merge unrelated histories
git pull origin develop --allow-unrelated-histories

# lost track of remote branch
git branch --set-upstream-to=origin/develop develop
```
[back to top](#git)

## References
- https://git-scm.com/docs/
- https://nvie.com/posts/a-successful-git-branching-model/
- https://github.com/nvie/gitflow
- https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow
- https://www.google.com/search?q=git+flow
- https://stackoverflow.com/questions/1057564/pretty-git-branch-graphs

[back to top](#git)
