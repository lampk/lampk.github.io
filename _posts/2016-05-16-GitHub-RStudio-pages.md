---
layout: post
title: Set up GitHub repository and its website with RStudio
status: publish
published: true
---
 
# Create GitHub repository and set up with RStudio
 
Based on: [http://www.r-bloggers.com/rstudio-and-github/](http://www.r-bloggers.com/rstudio-and-github/)
 
* Step 1: create repository on GitHub (New repository)
* Step 2: create new project with RStudio
* Step 3: make initial commit
* Step 4: connect RStudio project and GitHub repository
    + Open Shell
    + Use commands: 
    
```
git remote add origin https://github.com/username/repository
git config remote.origin.url
git pull origin master
git push origin master
```
    
# Set up website for GitHub repository with RStudio
 
Based on: 
    + [https://gist.github.com/cobyism/4730490](https://gist.github.com/cobyism/4730490) and 
    + [http://www.damian.oquanta.info/posts/one-line-deployment-of-your-site-to-gh-pages.html](http://www.damian.oquanta.info/posts/one-line-deployment-of-your-site-to-gh-pages.html)
 
* Open Shell
* Use commands:
 
```
git checkout master
git subtree split --prefix output -b gh-pages
git push -f origin gh-pages:gh-pages
git branch -D gh-pages
```    
    
