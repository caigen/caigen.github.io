---
layout: post
title: "How to remember the git workd flow?"
date: 2014-03-09 14:16:00
---

###I, git basic work flow.

1, From 0 to 1.

    git init

2, How is everything going?

    git status
	
3, With 'git status', we could know the untracked files. As we know, git is used to track the files. So now we wanna git to track the untracked files.
    
    git add <file_name>

4, Now the file is tracked, that is to say, we have made some change. We wanna git to keep this change and update the status.

    git commit -m 'the change message.'

5, After things are done, we wanna to get some info about the past changes. The next command could show the *commit* log.

    git log

###II, git local <-> remote work flow.

1, From 0 to 1, where is the remote?

    git remote add <name> <url>

2, Mow we could transfer the update between local and remote repo.

    git push origin master
    git pull origin master ~= git fetch orgin & git merge master

###III, git branch & merge work flow.

1, First of all, we should create a new branch.
    
    git branch <branchname>

2, Now we wanna work under the new branch. We could treat the branch as a place we want to buy something and pay for it. So now we 'checkout' at the 'branch'. 

    git checkout <branchname>

3, After you do some necessage change, we checkout back to previous branch.
    
    git checkout master

4, Sync the changes. Remember that now we are at *master* location.
    
    git merge <branchname>

5, So now the new branch we created before is useless, just delete it.
    
    git branch -d <branchname>

###References
[1] try-git: *[http://try.github.io/levels/1/challenges/1](http://try.github.io/levels/1/challenges/1)*
