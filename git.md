# GIT

SCM: Software Configuration Management

1. [Configure](#configure)
2. [Repository](#repository) 
	1. [Local Repository](#local-repository)
	2. [Remote Repository](#remote-repository)
3. [Commit Changes](#commit-changes)
4. [Branches](#branches)
5. [Tags](#tags)
6. [Git Flow Workflow](#git-flow-workflow)
	1. [Develop branch](#develop-branch)
	2. [Feature branch](#feature-branch)
	3. [Feature branch with Pull Request](#feature-branch-with-pull-request)
	4. [Release branch](#release-branch)
	5. [Hotfix branch](#hotfix-branch)
7. [Common errors](#common-erros)

----
## Configure

Local configurations

```bash

git config ${location} ${option} ${configuration} "value"

```

Configurations:

| location | Option | Configuration | Description |
|---|---|---|---|
| --global |  | user.name | user name  |
| --global |  | user.email | user email address |
| --global |  | core.pager | tool used to paginate (cat) |
| --global |  | merge.ff | enable/disable fast forward merge (no) |
| --global |  | core.editor | default editor (vi) |
| --global | --unset | user.name | remove user name |
| --global | -l |  | list global configurations |
| --local | -l |  | list local configurations  |
|  | -l |  | list all configurations |

Example:

```bash

git config --global user.name "usuario"
git config --global user.email "usuario@email.com"

git config --global --unset user.name

git config --global -l
git config --local -l
git config -l
```

Configure remote addresses

```bash

# list current configuration
git remote -v

# change from http to ssh (github)
git remote set-url origin git@github.com:${username}/${repository}.git

# change from ssh to http (github)
git remote set-url origin https://github.com/${username}/${repository}.git

```

[back to top](#git)

## Repository

### Local Repository

Create local repository

```bash

git init

```

### Remote Repository

Add and connect a remote repository with a local repository via SSH

```bash

git remote add origin ${remote_repository}.git
# Github
git remote add origin git@github.com:${username}/${repository}.git

# examples
git remote add origin git@github.com:username/my-repo.git
git push -u origin master

```

Clone remote repository via SSH

```bash

# to clone (download) a remote repository
git clone ${remote_repository}.git ${local_directory}
# Github
git clone git@github.com:${username}/${repository}.git ${local_directory}

# examples
git clone git@github.com:username/my-repo.git
git clone git@github.com:username/my-repo.git my-remote-repo

```

[back to top](#git)

## Commit Changes

Add new files

```bash

# new files need to be added to the index before committing them.
git add ${files}

# examples
git add .
git add *
git add *.txt
git add new-file.txt

```

[back to top](#git)

Remove added files

```bash

# to remove previusly added files before committing them.
git rm --cached ${file}
git rm --cached -r ${directory}

# examples
git rm --cached new-file.txt
git rm --cached -r target

```

[back to top](#git)

Display differences between the index and the current HEAD commit.

```bash

# displays untracked and uncommited files.
git status

# example message for untraked files
git status
#On branch master
#Untracked files:
#  (use "git add <file>..." to include in what will be committed)
#	new-file.txt

# example message for uncommited files
git status
#On branch master
#Changes to be committed:
#  (use "git restore --staged <file>..." to unstage)
#	new file:   new-file.txt

```

[back to top](#git)

Show differences between files before commit.

```bash

git diff ${option} ${branch_a} ${branch_b}

# examples
git diff 								# shows differences between local and HEAD
git diff --color-words					# shows colored differences
git diff --check						# checks if exist differences
git diff --stat 						# print stats 
git diff branch-a branch-b				# compare specified branches
git diff --color-words branch-a brach-b
 
```

[back to top](#git)

Stash changes for later use.

```bash

git stash ${command} ${option} ${index}
# if no command is specified, the push command is used.

# examples
git stash -u 					# stash current changes including untracked files.
git stash list 					# list existing stashes.
git stash show -p stash@{0} 	# compare specific stash with the last commit.
git stash pop 1					# restore stash by index.	
git stash drop 0				# drop stash by index.
git stash clear					# remove all stashes.

```

[back to top](#git)

Commit changes into local repository.

```bash

# commit current changes
git commit ${option}

# examples
git commit -a
git commit -am "commit message"
# to edit the message of the latest commit.
git commit --amend

```

[back to top](#git)

Push changes to remote repository.

```bash

git push ${option} ${local_origin} ${remote_destiny}

# examples
git push -u origin master	# push current branch to master, add upstream (tracking)
git push origin master		# push current branch to master
git push origin				# push current branch to configured upstream

```

[back to top](#git)

Log repository information.

```bash

git log $(options)

# examples
git log
git log --oneline

# show all the available information.
git show

```

[back to top](#git)

Restore, revert, reset and clean.

```bash

git restore .							# restore all files.
git restore '*.txt'						# restore all files with extension txt.
git restore file.txt					# restore file.txt.
git restore --source master~2 file.txt	# restore file.txt to two revisions back.

gir revert
git revert HEAD						# revert the latest commit and create a new commit.
git revert HEAD~3					# revert using a relative value -3 before the current state.
git revert ${commit_sha1}			# revert specific commit by id.
git revert -n ${commit_sha1}		# revert specific commit by id without commiting.
git revert --abort 					# cancel the current revert and returns to previous state.

git reset 							# by default the reset type is --mixed.
git reset HEAD 						# resert the latest commit.
git reset HEAD~2					# reset using a relative value -2 before the current state.
git reset ${commit_sha1}			# reset to the commit specified by id.
git reset --soft HEAD 				# reset to the specified commit but do not delete 
git reset --hard HEAD 				# reset to the specified commit and delete everyting after that commit. 
git reset --merge ORIG_HEAD 		# reset to the specified merge.

git clean							# remove untracked files.

```

[back to top](#git)

## Branches

Create a local branch

Create a remote branch

Push a local branch to a remote branch
[back to top](#git)

## Tags

Create a local tag

Delete a local tag

Push local tags to remote

Delete remote tags
[back to top](#git)

## Git Flow Workflow
### Develop branch

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

### Feature branch

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

### Feature branch with Pull Request

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

### Release branch

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

### Hotfix branch

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