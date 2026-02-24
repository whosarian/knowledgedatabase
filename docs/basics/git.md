# Git

Git is a distributed version control system.

## Architecture

Git doesnt store data as differences (like older systems do), but as snapshots.

- **Blobs**: Store the data of a file
- **Trees**: Stores the directory structure
- **Commits**: Point on a tree and story metadata

## Concept

1. **Working directory**: The local files youre working on
2. **Staging area**: Collecting the changes
3. **Repository**: Permanently saves all commits

## Basic commands

```bash
git init # create a new git repository in the current folder
git clone <url> # clone an existing repository from a server

git status # shows all changed files and whats in the staging area
git add <file> # stages changes to the staging area
git commit -m "Message" # creates a snapshot of your changes with a description
git log # shows the history of all commits

git pull # pulls new changes from the server and brings them together with your code
git push # pushes your local commits to the server

git branch <name> # create a new branch
git checkout <name> # changes the branch your working in
git merge <name> # brings together the changes of two branches
```

## Advanced commands

```bash
git commit --amend # changes the last commit
git reset --soft HEAD~1 # reverts the last commit but holds the changes in the staging area
git reset --hard # deletes all unsaved changes
git cherry-pick <hash> # copies a single commit from a diffent branch in your current branch
git rebase -i # interactive rebase: allows squashing or renaming of old commits
```

## `.gitignore`

In this file you can set files or directories, that should be ignored by git:

- Temporary files
- OS files
- Secrets and keys

## Good practices

1. Small but more commits
2. Meaningful commit messages
3. New branch for every new feature

## Gitflow

Gitflow is a branching model that uses the following branches:

- `main`: Stable production branch
- `develop`: Main developing branch where new features are included in
- `feature`: Temporary branches to develop new features
- `release`: Branches to prepare releases

![GitFlow](/docs/images/gitflow.svg)
