# Everyday git commands

### Remove files from *Changes to be committed:*

```git restore filename --staged```


### Remove files from *Changes not staged for commit:*

```git checkout filename```

### Remove files from *Untracked files:*

```git clean -f filename```


### Change latest pushed commit:

1) ```git reset sha1 or label``` (choose before our latest commit). This command doesn't remove any local changes you had done before pushing to remote.

2) Do your changes (probably using steps above)

3) ```git commit -m "Your comment"```

4) ```git push -f origin your branch```
