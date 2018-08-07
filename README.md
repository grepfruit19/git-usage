This is just a reference for git things that are useful to me, and maybe useful to others too.

Beyond the typical clone/push/pull commands, git has a few extra commands that are useful to know.

### Set up an SSH key

This will make it so you don't have to type in your password anywhere. This is well documented so just google `github ssh key`.

### git log

Shows all commits on your current branch. Note the hashes next to each commit, those are unique to each commit and identifies each commit.

### git reflog

### git reset

Git reset comes in two flavors, hard resets and soft resets, and takes in a parameter of a commit hash

Take a list of commits like this, in reverse chronological order.

```
Commit 5: d4166aed7c15b586f2a0b5f0c6c1d9a1056bdef3
Commit 4: b191b0b7581e99ab0c1e1bfe050a892ea91d7455
Commit 3: 623b68e9c1c7bea9945fafc88ece8b7d5be7b6c4
Commit 2: 0120e2c7b01a2e4b6618d2d2e2449b53c2369120
Commit 1: e89084bbb16bf608580fe454cc78dd1e96d5cbae
```

**Soft resets** revert to the given commit, and unstage any changes that were made by later commits. So if you ran `git reset 623b68e9c1c7bea9945fafc88ece8b7d5be7b6c4`, then you would revert to commit 3, and any changes made by commits 4 or 5 would still be present, but would be unstaged. You can then do something like create a new commit to override them, a bit easier than a cherry pick in my opinion.

**Hard resets** revert to the given commit and remove any changes that were made by later commits. If you run `git reset --hard 623b68e9c1c7bea9945fafc88ece8b7d5be7b6c4`, then you would revert to commit 3 and any changes in commits 4 or 5 will be removed completely. This is a destructive command.

### Force push

Sometimes your branch and the upstream branch have diverged and you don't want to deal with merging or rebasing. Sometimes you don't care about what's upstream, you know that whatever's on your branch is what needs to be up there. Whatever the reason, you can do a force push to have the upstream accept your changes by adding a plus to the branch, like so `git push origin +myBranch`.

#### A note about rewriting history

When you do anything on git beyond appending commits, you are rewriting history (e.g., removing commits, force pushing, cherry picking). Generally, you should never do this on a branch that multiple people are using, or at least you should let them know, lest ye risk some serious git headaches.
