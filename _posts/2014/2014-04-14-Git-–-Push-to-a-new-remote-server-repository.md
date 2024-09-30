---
title: "Git – Push to a new remote server repository"
date: "2014-04-14T17:33:19"
tags: [
  "development",
  "technical",
  "tools"
]
---
Short reminder to myself on how to create a new server repo and do the initial push to it from a workstation.

**On the Server:**

> cd Repositories  
> mkdir Project.git  
> git init –bare

**On the Workstation:**

> cd Project  
> git init  
> git add \*  
> git commit “Initial commit”  
> git remote add origin username@server.com/disk/shares/repositories/project.git  
> git push –u origin master

Done.