---
title: "Safe removal of previous commits"
categories:
  - git-github
tags:
  - git
  - github
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
---
## How to unstage your previous commits (safely)
You can see every commit on you branch by:
```
$ git log
```
and exit the log by `q`

I have a very very painful experience with this one:
```
$ git reset --hard HEAD~2
```
I just simply Googled 'how can i delete previous commit' and got answer from Stackoverflow
which was `git reset --hard HEAD~(number of commits you want to delete)`.  
Without any hesitation i just typed the command and here came the disaster!! BOOM! Every file I made
through the commit, which i just reset before, was totally gone! OK... learning by doing, learning by mistakes...  

```
$ git reset --soft HEAD~2
```
Guys, this is the safest way to remove your previous commits, as you can infer from the argument `--soft`.
If you want to keep your work/files in your local directory or remote repo, and just undo the commit you made before,
do `git reset --soft HEAD~n`.  

### Then when do you actually use git reset --hard?
Of course you can use it when you really want to reset your code because your code was like such a garbage.  
However, recently, I found a really good use case of hard resetting while I was co-working on a project through git and github.
When you work with many people through git, it is quite common that you mess up with push timing issue. 
You worked on one part and tried to push it, but you see that other members have already pushed their changes before. 
Normally, since you want to save your changes, you can `stash` your changes and then `pull` the remote changes.  
However, if you want your changes to be overwritten by other members' changes, you can `reset hard` your code and then `pull` the remote changes.  

Summarized: **You can hard reset your code when you want to overwrite your changes**