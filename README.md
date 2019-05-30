# git_merges

![network](https://user-images.githubusercontent.com/25508038/58631259-4d24fc00-82e2-11e9-916b-f022437af472.PNG)

```bash
history
```

## setup 

```bash
$ git init
$ git add .
$ git commit -m "initial commit"
[master (root-commit) c9c2f93] initial commit
 1 file changed, 1 insertion(+)
 create mode 100644 test.txt
 
$ git remote add origin https://github.com/JanMalch/git_merges.git
$ git push -u origin master
Enumerating objects: 3, done.
Counting objects: 100% (3/3), done.
Writing objects: 100% (3/3), 211 bytes | 211.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/JanMalch/git_merges.git
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.
```

## fast-forward merge

no additional commits created

fast-forward auto-merge

```bash
# creating branch

$ git checkout -b hotfix
Switched to a new branch 'hotfix'

$ git add .
$ git commit -m "hotfix"
[hotfix 54d56c1] hotfix
 1 file changed, 2 insertions(+), 1 deletion(-)
 
$ git push -u origin hotfix
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Writing objects: 100% (3/3), 246 bytes | 246.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/JanMalch/git_merges.git
 * [new branch]      hotfix -> hotfix
Branch 'hotfix' set up to track remote branch 'hotfix' from 'origin'.

# merging into master

$ git checkout master
Switched to branch 'master'
Your branch is up to date with 'origin/master'.

$ git merge hotfix
Updating c9c2f93..54d56c1
Fast-forward
 test.txt | 3 ++-
 1 file changed, 2 insertions(+), 1 deletion(-)
 
$ git push
Total 0 (delta 0), reused 0 (delta 0)
To https://github.com/JanMalch/git_merges.git
   c9c2f93..54d56c1  master -> master

$ git status
On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean
```

## diverging branches

automated commit for merging second branch

fast-forward auto-merge for first branch, 'recursive strategy' auto-merge for second branch
>commit [4034054](https://github.com/JanMalch/git_merges/commit/403405456c1efd65de9a82d3e4151cae5ff17e17), 2 parents [25295e3](https://github.com/JanMalch/git_merges/commit/25295e304879b0017d3df3e62e7b4cbce3c2ba65) + [c8983db](https://github.com/JanMalch/git_merges/commit/c8983db778c8089fdab6204f51636a1596de7ec3)

```bash
# creating first branch

$ git checkout -b diverge/a
Switched to a new branch 'diverge/a'

$ git add .
$ git commit -m "diverge a"
[diverge/a 25295e3] diverge a
 1 file changed, 2 insertions(+), 1 deletion(-)
 
$ git push -u origin diverge/a
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Writing objects: 100% (3/3), 258 bytes | 258.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/JanMalch/git_merges.git
 * [new branch]      diverge/a -> diverge/a
Branch 'diverge/a' set up to track remote branch 'diverge/a' from 'origin'.

# creating second branch

$ git checkout master
Switched to branch 'master'
Your branch is up to date with 'origin/master'.

$ git checkout -b diverge/b
Switched to a new branch 'diverge/b'

$ git add .\b-file.txt
$ git commit -m "diverge b"
[diverge/b c8983db] diverge b
 1 file changed, 1 insertion(+)
 create mode 100644 b-file.txt
 
$ git push -u origin diverge/b
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 6 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 281 bytes | 281.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/JanMalch/git_merges.git
 * [new branch]      diverge/b -> diverge/b
Branch 'diverge/b' set up to track remote branch 'diverge/b' from 'origin'.

# merging into master

$ git checkout master
Switched to branch 'master'
Your branch is up to date with 'origin/master'.

$ git merge diverge/a
Updating 54d56c1..25295e3
Fast-forward
 test.txt | 3 ++-
 1 file changed, 2 insertions(+), 1 deletion(-)
 
$ git push
Total 0 (delta 0), reused 0 (delta 0)
To https://github.com/JanMalch/git_merges.git
   54d56c1..25295e3  master -> master
   
$ git merge diverge/b
Merge made by the 'recursive' strategy.
 b-file.txt | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 b-file.txt
 
$ git push
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 6 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (2/2), 302 bytes | 100.00 KiB/s, done.
Total 2 (delta 0), reused 0 (delta 0)
To https://github.com/JanMalch/git_merges.git
   25295e3..4034054  master -> master
```

## diverging branches with conflict

no automated commit for merging second branch. manual conflict resolution instead 

fast-forward auto-merge for first branch, no auto-merge for second branch

>commit [703f45e](https://github.com/JanMalch/git_merges/commit/703f45e55d5688ce813a2b6df187f758cff7fa0f), 2 parents [3481224](https://github.com/JanMalch/git_merges/commit/3481224ee2c60a28d517f3856bf3f88728e9402b) + [ac1a760](https://github.com/JanMalch/git_merges/commit/ac1a76061b83f59e935582c3bdbd202c6a0db7d8)

```bash
# creating first branch

$ git checkout -b diverge/a-conflict
Switched to a new branch 'diverge/a-conflict'

$ git add .
$ git commit -m "diverge a - conflict"
[diverge/a-conflict 3481224] diverge a - conflict
 1 file changed, 2 insertions(+), 1 deletion(-)
 
$ git push -u origin diverge/a-conflict
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 6 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 308 bytes | 308.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/JanMalch/git_merges.git
 * [new branch]      diverge/a-conflict -> diverge/a-conflict
Branch 'diverge/a-conflict' set up to track remote branch 'diverge/a-conflict' from 'origin'.

# creating second branch

$ git checkout master
Switched to branch 'master'
Your branch is up to date with 'origin/master'.

$ git checkout -b diverge/b-conflict
Switched to a new branch 'diverge/b-conflict'

$ git add .
$ git commit -m "diverge b - conflict"
[diverge/b-conflict ac1a760] diverge b - conflict
 1 file changed, 2 insertions(+), 1 deletion(-)
 
$ git push -u origin diverge/b-conflict
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 6 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 309 bytes | 309.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/JanMalch/git_merges.git
 * [new branch]      diverge/b-conflict -> diverge/b-conflict
Branch 'diverge/b-conflict' set up to track remote branch 'diverge/b-conflict' from 'origin'.

$ git checkout master
Switched to branch 'master'
Your branch is up to date with 'origin/master'.

# merging first branch into master

$ git checkout master
Switched to branch 'master'
Your branch is up to date with 'origin/master'.

$ git merge diverge/a-conflict
Updating 4034054..3481224
Fast-forward
 test.txt | 3 ++-
 1 file changed, 2 insertions(+), 1 deletion(-)
 
$ git push
Total 0 (delta 0), reused 0 (delta 0)
To https://github.com/JanMalch/git_merges.git
   4034054..3481224  master -> master
   
# merging second branch into master

$ git merge diverge/b-conflict
Auto-merging test.txt
CONFLICT (content): Merge conflict in test.txt
Automatic merge failed; fix conflicts and then commit the result.

$ git status
On branch master
Your branch is up to date with 'origin/master'.

You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)

        both modified:   test.txt

no changes added to commit (use "git add" and/or "git commit -a")

# resulting conflict file was copied manually as test-conflict.txt
# conflict in test.txt was resolved manually

$ git add .
$ git commit -m "resolve conflict"
[master 703f45e] resolve conflict

$ git push
Enumerating objects: 1, done.
Counting objects: 100% (1/1), done.
Writing objects: 100% (1/1), 217 bytes | 217.00 KiB/s, done.
Total 1 (delta 0), reused 0 (delta 0)
To https://github.com/JanMalch/git_merges.git
   3481224..703f45e  master -> master
```


