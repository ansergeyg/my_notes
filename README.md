# Everyday git commands

### Remove files from *Changes to be committed:*

```git restore filename --staged```


### Remove files from *Changes not staged for commit:*

```git checkout filename```

### Remove files from *Untracked files:*

```git clean -f filename or leave empty to delete all untracked files```


### Change latest pushed commit:

1) ```git reset sha1 or label``` (choose before our latest commit). This command doesn't remove any local changes you had done before pushing to remote.

2) Do your changes (probably using steps above)

3) ```git commit -m "Your comment"```

4) ```git push -f origin your branch```

## Git reset
 
Use git reflog to see all manipulations you did locally with git commit history.

```git reflog```

### Switch to a specific commit

```git reset HEAD~N```

Where *N* is a number of commits that will be cut from the HEAD

or

```git reset commit-hash```

### Cancel git reset command

Find the proper commit in git reflog you want to move to and then run:

```git reset commit-hash```

or

```git reset HEAD@{N}```

where *N* is a number you see in git reflog. Example: HEAD@{0}

### Divergent branch issue

Say, you were in the develop branch and then commited some changes and after that created a new branch.

When you get back to the develop branch and run git pull, you get the "divergent branches" issue.

You cannot really rebase or fast-forward because you local branch has commits that are not present in the remote branch AND the remote branch has some changes that are not present in you local branch.

In this situation, you can just run 

```git reset --hard origin/develop```

to reset your branch to develop. You don't care about the local commits because they are saved in the new branch that you had already created and worked on. 
