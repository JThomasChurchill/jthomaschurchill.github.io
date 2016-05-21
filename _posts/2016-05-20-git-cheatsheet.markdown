---
layout: post
title:  "Git Cheatsheet"
date:   2016-05-20 10:00:00
categories: Git
css_class: "tutorial"
---

<style>
  h3 {color: #4a549c;}
  img {box-shadow: 0px 0px 5px darkgray;}
  table {margin-bottom: 15px;}
  table caption {color: #58acfa; font-weight: bold;}
  th, td, tr {background-color: #fff; border: 1px solid #999; padding: 5px;}
</style>

---

1. [Create a new repository](#create-a-new-repository)
1. [Clone a GitHub Repo](#clone-a-github-repo)
1. [Check Status](#check-status)
1. [Staging And Unstaging Files](#staging-and-unstaging-files)
1. [Commit Changes](#commit-changes)

---

## Set Up

---

### Interact with your Github Account

```
git config --global user.name "YourUserName"
git config --global user.email "YourEmail"
```

---

### Create a New Repository

```
git init
```

### Clone a GitHub Repo

Copy the URL for the repo from GitHub.

![clone a github repository screenshot](/images/github_clone.png)

---

In a terminal, cd into the target directory and clone the repo.

```dos
git clone https://github.com/YOUR-USERNAME/YOUR-REPOSITORY
```

---

## Check Status

---

### Check Current Status

Checking status tells you:

* Which branch you are on
* if there are changes that have not been staged 
* if there are uncommitted changes that have already been staged
* if there are new files that need to be added
* probably other stuff too, I don't know, leave me alone

```
git status
```

---

### Checking Differences

```
git diff -- file_to_check
```

---

## Making Changes

You have to "stage" changes you have made before you can commit them. In this way you can select a subset of all the modified files to be committed with the same commit message.

---

### Staging And Unstaging Files

You stage a file the same way you add a new one by using the "add" command. If you modify a file after staging it, you can still commit it, but only the changes that were staged will be committed. Any changes made after staging will have to also be staged to be committed.

```
git add my_new_or_modified_file
```

You can unstage a file with the reset command.

```dos
git reset HEAD your_file
```

---

### Commit Changes

Once you have "added" (aka "staged") your modified files, you can commit them.

```
git commit -m "This is a commit message"
```

If you are connected to a GitHub account you can push your local changes to GitHub. You will be prompted for you username and password.

```dos
git push origin master
```
