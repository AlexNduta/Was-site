+++
title = "Git Branching"
date = "2024-01-25"
author = "Alex Kinyanjui"
+++


- Git branching is a feature provided by git that allows you to separate and contain different workflows in your project  in seperate `branches`  steming from a central branch commonly known as the `main` branch. You can then merge each workflow branch back to the main branch.
- Even though git lets you manage your own branching,  there are two main branching strategies that commonly use:
	- GitFlow
	- GiHubFlow

1. # GitFlow
- This is a Git branching  model that involves having feature branches and multiple primary branches
- compared to trunk-based branching, gitflow has many long-lived branches and larger commits
- A long long-lived branch is a branch that stays throughout the project's lifecycle.
-  Developers create a feature branch and delay merging into main branch until the feature is complete.
-  It is ideal for projects that have a scheduled release cycle


### How it works
![alex](/img/al.png?width=200&height=100)

- instead of having one `main` branch, the workflow uses two branches to record the workflow of the project i.e the `main` and the `develop`
- The `main` branch stores the official release histoty versioned by number and the `develop` branch serves as the integration branch for features.
- we initially start with an empty `develop` branch  localy which is later pushed to the server
```sh
git branch develop

git push -u origin develop

```
- This branch will contain the complete history of the project, `main` will be a summary version
- other devs will clone the central repo and create a tracking branch for `develop`
- [`git-flow extension`](https://skoch.github.io/Git-Workflow/) exists to make it easier to run a single command to do a task rather than using multiple commands as it is intended in vanilla git.
 - if you are using `git-flow` extension, running `git flow init` on an existing repository creates a developer branch


 ## Feature branches
- Each new feature should reside in its own branch which can be pushed to `develop` which acts as their parent branch
- when a feature is complete, it gets mearged into the `develop` branch
- `feature` branches should never interact with the main branch
- feature branches are created from the latest  `develop` branch

## creating a feature branch
- without the git-flow extension
```sh
git checkout develop
git checkout -b feature_branch # create a feature from the develop
```

- using git flow extension
```sh
git flow feature start feature_branch
```

## mearging the branch
- when you are done with the feaure branch, you meage it into `develop` branch
```sh
git checkout develop
git merge feature_branch
```
- using git-flow extension
```sh
git flow feature finish feature_branch
```


## Release branches
- Once `develop` has enough features or the release date is close, developers fork a release branch off `develop`
 - Forking this branch starts the release cycle, meaning that no new features are added, just bug fixes
 - Once its ready to ship, the release branch is meaged to `main` and tagged with a version number
 - without git-flow extensions
```sh
# creating release branches
$ git checkout develop

$ git checkout -b release/0.1.0
```
- Using git-flow extensions
```sh

$ git flow release start 0.1.0
switched to a new branch  'release/ 0.1.0'
```
- Once a release is ready to ship, te `release` branch is merged to `main` and deleted
```sh
# mearge without git-flow extension
$ git checkout main
$ git merge release/0.1.0


# using git-flow extension
$ git flow release finish '0.1.1'
```

## Hotfix branch
- these are maintenance branches used to patch production releases
	- They are like `release` or `feature` branches just that they are used only on  `main`  branch instead of `develop` and they are short-lived.
- As soon as a fix is done, it should be mearged into `main` and develop

```sh
# without git-flow extension
$ git checkout main
$ git checkout -b hotfix_branch


# using git-flow extension
$ git flow hotfix start hotfix_branch
```

- mearging a finished fix branch into `main`
```sh
$ git checkout main
$ git merge hotfix_branch
$ git checkout develop
$ git merge hotfix_branch
$ git branch -D hotfix_branch

# using git extension
$ git flow hotfix finish hotfix_branch
```

![70a2c4ab7b26a6529fdd7f354e6db32e.png](:/27b2831a436641aba248570677b065c9)

### Git flow extension
**installation**
`apt-get install git-flow`


# 2. GitHub Flow (feature branch flow)
- This is a simple setup that encourages the use of two branches:
	- The working branches i.e develop, feature, release, hotfix etc..
	- main branch
- Developers create a `feature` branch for every feature or bug fix which is later  merged into `main` or `develop` after code review
