Consider the case we have faced:

You have a develop branch. In that branch you have a core.yml file that is updated automatically by some system.

One PR is going to merge a change in this core.yml file. Let's call this PR - PR1 

But there is another PR that doesn't have any changes to that core.yml file (no conflict at all). Let's call this PR - PR2

So then PR2 get's merged first. Ad it doesn't have anything todo with core.yml file it merges smoothly.

After that PR1 gets merged which brings changes to core.yml file.

And you know for sure that the changes should be there in the develop branch, but when you inspect the changes, they are not there.

Example of git history:

head

merge PR 3

some comit 9

some comit 8 (here we don't see any changes in core.yml file anymore. WHY?)

merge PR1 (this PR brings core.yml changes)

some commit 7 (core.yml change is here)

some comit 6

merge PR2 (suspected PR, but there were no changes touching core.yml file!)

some comit 5

some commit 4

some comit 3

merge PR0

some commit 2

some commit 1



https://stackoverflow.com/questions/1407638/git-merge-removing-files-i-want-to-keep
