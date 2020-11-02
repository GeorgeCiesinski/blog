---
layout: single
title:  "Git Commands"
date:   2020-11-02 00:21:00 -0500
categories: Programming Commands GitHub
comments: true
---

# Git Commands

## git init

Starts a new repository.

## git clone

Obtains a repository from an existing URL.

```
git clone (url)
```

## git remote

Connects your local repository to the remote server.

```
git remote add origin (url)
```

## git stash

**git stash** without any arguments is equivalent to git stash push.

```
git stash
```

**list** lets you view the list of stashes you made at any time.

```
git stash list
```

**save** temporarily stores all the modified tracked files. This has been deprecated in favour of `git stash push`. Does not accept messages.

```
git stash save
```

**apply** takes the top most stash in the stack and applies it to the repo.

```
git stash apply
```

**pop** restores the most recently stashed files and deletes the stash from the stack after it is applied.

```
git stash pop
```

When you git stash or git stash save, git creates a `git commit` object, and saves it in your repo.

**drop** deletes the latest stash from the stack. Probably impossible to revert.

```
git stash drop
```



**clear** deletes all the stashes made in the repo. Probably impossible to revert.

```
git stash clear
```

[More info](https://www.freecodecamp.org/news/useful-tricks-you-might-not-know-about-git-stash-e8a9490f0a1a/)

## git reset

Unstages the file, but preserves its contents.

```
git reset (file)
```

**--hard** moves the file to the latest commit and unstages the file.

```
git reset --hard (file)
```

## git rm

Deletes the file from your working directory and stages the deletion.

```
git rm (file)
```

## git log

Git log can be used to illustrate the repository, including the branches and ref names.

**--graph** draws a text-based graphical representation of the commit history

**--decorate** prints out the ref names of any commits shown

**--oneline** shorthand for "--pretty=oneline --abrev-commit" used together

**--all** Pretend as if all the refs in refs/, along with HEAD are listed on the command line as \<commit>

```
git log --graph --decorate --oneline --all
```

[More info](https://git-scm.com/docs/git-log)

# Additional Info

[Top 20 Git Commands](https://medium.com/edureka/git-commands-with-example-7c5a555d14c)