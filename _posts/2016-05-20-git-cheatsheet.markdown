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


## Set Up

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

```
git clone https://github.com/YOUR-USERNAME/YOUR-REPOSITORY
```

