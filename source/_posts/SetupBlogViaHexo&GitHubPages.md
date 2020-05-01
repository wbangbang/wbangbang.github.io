---
title: Set up Blog via Hexo & GitHub Pages
layout: post
tags:
  - Hexo
photos:
  - https://drive.google.com/uc?export=view&id=1tM2V4a4e4ZzliOxjTc6sO8la6MIpwfIF
---

This post is to show how to setup a personal blog via Hexo and GitHub Pages step by step.

<!--more-->


## Why Hexo

According to [this](https://stackshare.io/stackups/hexo-vs-jekyll):

> Some of the features offered by Hexo are:
> 
> * Blazing Fast - Node.js brings you incredible generating speed. Hundreds of files take only seconds to build.
> * Markdown Support - All features of GitHub Flavored Markdown are supported. You can even use most Octopress plugins in Hexo.
> * One-Command Deployment - You only need one command to deploy your site to GitHub Pages, Heroku or other sites.
> 
> On the other hand, Jekyll provides the following key features:
>
> * Simple - No more databases, comment moderation, or pesky updates to install—just your content.
> * Static - Markdown (or Textile), Liquid, HTML & CSS go in. Static sites come out ready for deployment.
> * Blog-aware - Permalinks, categories, pages, posts, and custom layouts are all first-class citizens here.

Hexo is powered by Node.js, while Jekyll is powered by Ruby. I'm not a fan of Ruby and I'm ramping up on Node.js, so that Hexo sounds more right.

## Setup 

Please ensure you have the following installed before installing hexo:

* Node.js (Should be at least Node.js 8.10, recommends 10.0 or higher)
* Git

You can easily install Hexo via npm:

```
$ npm install -g hexo-cli
```

This section only shows a simple installation. For more details or advanced installation, you may want to refer to [here](https://hexo.io/docs/#Installation).

```
$ hexo init <folder>
$ cd <folder>
$ npm install

```

For more details or advanced installation, you may want to refer to [here](https://hexo.io/docs/setup).

## Deployment

How to get your Hexo blog deployed on GitHub Pages? You may want to check the tutorial [here](https://hexo.io/docs/github-pages).

For Step#2, to push your local folder into GitHub, you can do the following:


```
$ cd <current_working_directory_of_your_local_project>

# Initialize the local directory as a Git repository.
$ git init

# Add all the files in your new local repository.
$ git add .

# Commit them.
$ git commit -m "First commit"

# Copy the remote repository URL and paste below to set it as remote
$ git remote add origin <remote_repository_URL>

# Verifies the new remote URL
$ git remote -v

# Push the changes in your local repository to GitHub.
$ git push origin master
```

([Reference](https://superuser.com/questions/1412078/bring-a-local-folder-to-remote-git-repo)).

Step#8~#10 are not accurate:

Please see a GitHub's document [About GitHub Pages](https://help.github.com/en/github/working-with-github-pages/about-github-pages):

> The default publishing source for user and organization sites is the master branch. If the repository for your user or organization site has a master branch, your site will publish automatically from that branch. You cannot choose a different publishing source for user or organization sites.

This being said, you cannot change the source. Instead, you need the following:

```
sudo: false
language: node_js
node_js:
  - 10
cache: npm
branches:
  only:
    - hexo-source # store source code of hexo in hexo-source branch
script:
  - hexo generate
deploy:
  provider: pages
  skip-cleanup: true
  github-token: $GH_TOKEN
  keep-history: true
  # added
  target_branch: master # generate static files to master
  on:
    branch: hexo-source # source code of hexo
  local-dir: public

```

And you need to create a new branch ```hexo-generate``` and use it to store your source code; and use ```master``` to store built artifacts. 

You may want to refer to [How do you track a remote branch?](https://www.git-tower.com/learn/git/faq/track-remote-upstream-branch) and [Make an existing Git branch track a remote branch?
](https://stackoverflow.com/questions/520650/make-an-existing-git-branch-track-a-remote-branch).

## Troubleshooting

### 