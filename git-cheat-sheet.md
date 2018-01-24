# Git cheat sheet

Based on refcardz from DZone (https://dzone.com/refcardz/getting-started-git).

## TL;DR

```
echo "# cheat-sheets" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/amioranza/cheat-sheets.git
git push -u origin master
```

## Setup credentials just for code credits not for authz or authn

```
git config --global user.name “amioranza”
git config --global user.email “amioranza@mdcnet.ninja”
git config --global color.ui “auto”
```

## Setting up the repository and first commit

```
git init
git add .
git commit –m 'The first commit'
```

## Clonning a repository

```
git clone <repo url>
```

- e.g.:

```
git clone git@bitbucket.org:rhyolight/javascript-data-store.git
```

## Treeish and Hash

Every Git commit generates a SHA-1 hash, 40 hex-characters. this allows the distributed work without the central server.

When browsing the repository you can use any portion of the generated hash to view the contents of the commit, 4-5 characters is enough on most cases.

Table of TREEISH

TREEISH | Definition
--- | ---
Head | HEAD The current committed version
HEAD^ | One commit ago
HEAD^^ | Two commits ago
HEAD~1 | One commit ago
HEAD~3 | Three commits ago
:/”Reformatting all” | Hybrid method of types 1, 2, and 3.
RELEASE-1.0 | User-defined tag applied to the code when it was certified for release.

## Getting some version based on the TREEISH shortcuts:

```
git log HEAD~3..HEAD
git checkout HEAD^^
git merge RELEASE-1.0
```

## Moving a file to a new place

```
git mv originalfile.txt newsubdir/newfilename.txt
```

## Removing a file

```
git rm fileyouwishtodelete.txt
```

## Status of the current repo

```
git status
```

## Diff between commits

```
git diff
git diff 32d4..
git diff --summary 32d4..
```

## Get changes since...

```
git log
git log --since=yesterday
git log --since=2weeks
```

## Blame the change author

```
git blame <filename>
```

## Temporarily return to the last commit

```
git stash
```

## Puting your stashed changes over your working copies

```
git stash pop
```

## Aborting the changes

```
git reset hard
```

## Restoring just one file from the previous commit

```
git checkout -- FileToRestore.go
```

## Adding files (staging)

```
git add <file name, folder name, or wildcard>
git add submodule1/PrimaryClass.java
git add .
git add *.java
```

## Adding files interactively

```
git add -i
```

## Adding selective changes

```
git add -p 
```

## Commiting changes

```
git commit
git commit -m "The commit message"
```

## Showing info about the last commit

```
git show
```

## Change the comments of the last commit without changing files

```
git amend
```

## Branching the code

```
git branch <new branch name> <from branch>
git branch <new branch name>
```

## Choosing a branch

```
git checkout <branch name>
```

## Listing branches

```
git branch -a
```

## Merging branchs

```
git merge <branch one>
git merge <branch one> <branch two>
```

## Rebase the code

```
git rebase <source branch name>
git rebase <source branch name> <destination branch name>
```

## Tagging your code

```
git tag <tag name>
git tag <tag name> <treeish>
```

## Viewing the remote repository

```
git remote -v
```

## Adding new remote to your local repo

```
git remote add <remote name> <remote address>
```

## Pushing your code to a remote repository

```
git push
```

## Fetch the code and merge to a branch

```
git fetch <remote name>
git merge <remote name/remote branch>
```

## Pulling code to refresh your local repo

```
git pull
git pull <remote name>
git pull <remote name> <branch name>
```

## Bundles to send files over USB or e-mail

```
git bundle create catchupsusan.bundle HEAD~8..HEAD
git bundle create catchupsusan.bundle --since=10.days master
```

## Listing or fetching from a bundle

```
git ls-remote catchupsusan.bundle
git fetch catchupsusan.bundle
```
