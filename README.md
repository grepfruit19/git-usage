This is just a reference for git things that are useful to me, and maybe useful to others too.

## Set up an SSH key

This will make it so you don't have to type in your password anywhere. This is well documented so just google `github ssh key`.

## git log

Shows all commits on your current branch. Note the hashes next to each commit, those are unique to each commit and identifies each commit.

It will produce something that looks like this:

```
Commit 5: d4166aed7c15b586f2a0b5f0c6c1d9a1056bdef3
Commit 4: b191b0b7581e99ab0c1e1bfe050a892ea91d7455
Commit 3: 623b68e9c1c7bea9945fafc88ece8b7d5be7b6c4
Commit 2: 0120e2c7b01a2e4b6618d2d2e2449b53c2369120
Commit 1: e89084bbb16bf608580fe454cc78dd1e96d5cbae
```

## git reset

Git reset comes in two flavors, hard and soft.

### Soft resets

Soft resets revert to the given commit, and unstages any changes that were made by later commits. So, referencing the `git log` output above, if you ran `git reset 623b68e9c1c7bea9945fafc88ece8b7d5be7b6c4`, then you would reset to commit 3. Any changes made by commits 4 or 5 would still be present, but would be unstaged. You can then do something like create a new commit to change your git history.

### Hard resets

Hard resets revert to the given commit and remove any changes that were made by later commits. If you run `git reset --hard 623b68e9c1c7bea9945fafc88ece8b7d5be7b6c4`, then you would reset to commit 3 and any changes in commits 4 or 5 will be removed completely. **Be careful with this, you can lose work doing this**

## git squash

The soft reset, commit trick earlier is my way of squashing commits. Here's another way using `git rebase`.

Let's say you want to squish your last 4 commits into a single one. Run `git rebase -i HEAD~4`, you'll get this in response.

```
$ git rebase -i HEAD~4

pick 01d1124 Monday commit
pick 6340aaa Tuesday commit
pick ebfd367 Wednesday commit
pick 30e0ccb Thursday commit
```

Note that this list is in chronological order (the opposite order of your `git log`).

To squish Tuesday into Monday, you want to edit it into this:

```
pick 01d1124 Monday commit
fixup 6340aaa Tuesday commit
pick ebfd367 Wednesday commit
pick 30e0ccb Thursday commit
```

I know we said squash, and we're using `fixup`, but `fixup` is the same as `squash`, but will remove log messages. It's like that commit never happened. That will squish Tuesday's commit into Monday (squishing merges commits into the _previous_ commit). If you want to rename Monday, you can edit it like this:

```
reword 01d1124 A reasonable commit message
fixup 6340aaa Tuesday commit
pick ebfd367 Wednesday commit
pick 30e0ccb Thursday commit
```

This will squish Tuesday's commit into Mondays, and rename it `A reasonable commit message`.

## A note about rewriting history

When you do anything on git beyond adding commits, you are rewriting history (e.g., removing commits, force pushing, squashing). Generally, you should never do this on a branch that multiple people are using, or at least you should let them know, otherwise you risk creating some serious git headaches.

## git rebase

Rebasing a branch applies changes from one branch on top of another branch. You will often need to do this if the main branch has had updates since you started your work, and you need to update your PR.

Imagine you forked your branch from main at commit E, like below

```
          A---B---C topic
         /
    D---E---F---G main
```

main has had additional commits since yours, so you want to rebase (pull in the latest main commits and then apply your commits on top)

```
                  A'--B'--C' topic
                 /
    D---E---F---G main
```

You can do this by running `git rebase main topic`, or in other words, `git rebase <branch to pull in> <branch with your changes>`.

An alternative command to this is `git pull --rebase origin <branch to pull in>`.

This command will apply your commits one at a time. If there are any merge conflicts, you will need to resolve them, `git add` whatever file had the conflict, and then run `git rebase --continue` (or --skip in certain situations).

### big rebases

Rebasing will apply your commits one at a time, and if you have a lot of commits, you may need to resolve a LOT of merge conflicts. In order to get around this, you can squash your commits together so you only have to merge one in.

## Misc stuff

### git cherry-pick

If you need to pull a single commit from another branch, you can `git log` to get that commit's commit hash, and then switch to the branch you need to pull into and run `git cherry-pick <commit hash>`.

### Combine changes into your last commit

If you forgot to make a tiny change that you want to include in your last commit, you can use

`git commit --amend --no-edit`, and that will just shove your staged changes into your last commit.

### Force push

Sometimes your branch and the upstream branch have diverged and you don't want to deal with merging or rebasing. Sometimes you don't care about what's upstream, because you know that whatever's on your branch is what needs to be up there. Whatever the reason, you can do a force push to have the upstream accept your changes by adding a plus to the branch, like so `git push origin +myBranch`. **Don't do this on a branch multiple people are using without warning them**, you will cause serious git headaches.

You will often need to do this if you're rebasing.

### Force pull

If you don't care about what's on your branch, and you just want whatever's on the remote, run `git fetch --all` (to update your local git so that it knows what's on the remote) and then run `git reset --hard origin/<branch name>`. This is a destructive command.

### git reflog

The reflog is similar to the log but it contains a list of (almost) every change that took place in your local repository. If you accidentally committed over something or lost something due to a destructive command, you may be able to find it in the reflog.
