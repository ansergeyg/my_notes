If you need to migrate some files/code/gittag from one repository to another

you can follow this scenario:

```
git clone gitpath from_repository (or you're already there)
```

```
git add remote to_repository gitpath
```

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
git push to_repository
```

Migrate tags:

