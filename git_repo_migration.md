If you need to migrate some files/code/gittag from one repository to another

you can follow this scenario:

1)
```
git clone gitpath from_repository (or you're already there)
```

2)
```
git add remote to_repository gitpath
```

3)
```
git pull from_repository
git pull to_repository
```

```
git branch (you should see all the branches from both repositories)
```

make your changes and save them in stage

```
git add
git commit
```

```
git push to_repository
```

Migrate tags:

Basically the same
