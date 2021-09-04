---
layout: post
title: "Node notes"
date: 2021-09-04 11:00:00 +0100
categories:
---

- [npm](#npm)
- [npx](#npx)
- [yarn](#yarn)
- [nvm](#nvm)


## npm

Node package manager is installed as part of the node installation.


- Install a package locally:

```
npm install lodash
```

Package is installed into the local _node_modules_ directory.


- Install a package globally:

```
npm install -g lodash
```

Package is installed into the global root.


- View global root (where global packages are installed):

```
npm root -g
```

## npx

Npx is an npm package runner. It will search for packages in your local and the global registry and run them if found.  If the package is not found it will download and install it first.

```
npx npm-remote-ls bower
```


## yarn

Is alternative package management tool to _npm_ it was created (2016) to address some issues with npm at that time.  Namely it:

- Install packages in parallel
- Creates a lock file.

__N.__ These issues have mainly been addressed within npm now. 


## nvm

Node version manager allows you to install and upgrade node

- Install the most recent long term stable:

```sh
nvm install –lts
```

- List the versions available:

```sh
nvm ls-remote
```

- Install nvm:

```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash
```


