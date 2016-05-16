---
layout: post
title: Update R packages with RStudio in Mac
status: publish
published: true
---
 
# Update R packages with RStudio in Mac
 
* Based on: http://stackoverflow.com/questions/13656699/update-r-using-rstudio
 
## Step 1: update R
## Step 2: update RStudio
## Step 3: move the packages from the old R installation into the new version
* From: /Library/Frameworks/R.framework/Versions/X.XX/Resources/library
* To: /Library/Frameworks/R.framework/Versions/X.XX/Resources/library
 
## Step 4: update your packages by typing update.packages() in your RStudio console, and answering 'y' to all of the prompts.
## Step 5: check 
* > version
* > packageStatus()
    
    
