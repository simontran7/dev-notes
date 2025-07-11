# Git

## Setup

```
git init <directory>
```

Create an empty Git repository in `<directory>`.

## Cloning & Remote

```
git clone <repo-url>
```

Clone repository at `<repo-url>` onto the local machine.

```
git remote -v
```

Show current remotes and their fetch/push URLs.

```
git remote add <remote> <repo-url>
```

Add a new remote named `<remote>` pointing to `<repo-url>`.

## Staging & Committing

```
git add <file-or-directory>
```

Stage changes in `<file-or-directory>` for the next commit.

```
git commit -m "<message>"
```

Commit staged changes using `<message>` as the commit message.

```
git status
```

List which files are staged, unstaged, and untracked.

## Stashing

```
git stash
```

Temporarily save uncommitted changes.

```
git stash pop
```

Re-apply the last stashed changes and remove it from stash.

## Branching

```
git branch
```

List all local branches and highlight the current one.

```
git branch <branch>
```

Create a new branch named `<branch>`.

```
git switch <branch>
```

Switch to the branch `<branch>`.

```
git switch -c <branch>
```

Create and switch to a new branch named `<branch>`.

## Pushing & Pulling

```
git push <remote> <branch>
```

Push committed changes to `<remote>` on `<branch>`.
Creates `<branch>` on the remote if it doesn’t exist.

```
git pull <remote> <branch>
```

Fetch and merge changes from `<branch>` on `<remote>` into the current branch.

## Viewing History

```
git log --oneline --graph --all
```

Show a condensed visual commit history of all branches.

```
git diff
```

Show unstaged changes between working directory and the index.

## Non-destructive Changes

TO DO

## Destructive Changes

```
git commit --amend
```

With nothing staged, edit the last commit's message.
