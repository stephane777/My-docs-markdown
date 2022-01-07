# GIT DIFF

`Git diff` show the difference between unstaged and last commit

`git diff HEAD` show the difference between unstaged, stagged and HEAD

`git diff --staged` or `--cached` show the difference between staged and last commit

`git diff HEAD [filenames]`
ex: `git diff HEAD main/style.css index.html`

`git diff branch1..branch2` // order matters !\
`git diff branch1 branch2` // is equivalent to the above

`git diff 4a9da7b..2353221` // comparaison between 2 commits\
`ex: git diff HEAD HEAD~1` // comparing commit between HEAD and previous commit

# GIT LOG

`git log` // display the history of commit for the current branch\
`git log` --oneline // display only the hash and the 1 line of the commit

# GIT STASH

When creating a new branch from master for example and if we add new changes on this new branch and if we want to go back to the\
master branch:
=> the 1st solution git will switch to master branch with the new change if there is no confict !
=> the 2nd solution is to stash the uncommited change ( staged or unstaged ), it will remove them from the working directory and staged area.\
It will allow you now to switch easily between branches without any warning. Once done to re-apply the change run `git stash pop`

`git stash` // stash uncommited change ( change from working directory or staged area )\
`git stash save` // similar to above\
`git stash pop` // add files back to working directory and remove all files from the stash\
`git stash apply` // similar to git stash pop: move files from stash to working directory and staged but all files remain in the stash and\
they can be applied on other branch.

`git stash list` // we can add multiple stash in time and can be listed.\
`git stash apply stash@{2}`

`git stash drop stash@{2}` // remove this particular stash\
`git stash clear` // remove all stash

# GIT CHECKOUT, RESTORE, RESET & REVERT

`git checkout 53c84e87` // Detached HEAD from master\
`git checkout master` // HEAD is now on master

`git checkout HEAD~1` // one commit before HEAD\
`git checkout -` // similar to git checkout HEAD or git checkout branch

`git checkout HEAD filename` // will remove all the change and we will have everything the same where HEAD is.\
`git checkout -- filename` // similar to above

`git restore --source HEAD~1 cat.txt` // it will replace the current file with the file how it was at HEAD~1\
// this will create a change in the working directory for that file and need to be commited.

`git restore filename` // undo/remove change for the filename in the working directory\
`git restore --staged filename` // remove the file from the staging area.

`git reset <commit-hash>` // it will remove/delete all commit till commitHash in the command\
// all the change in that commit will appear in the working directory\
`git reset --hard <commit-hash>` // same as above except that all the change in the commit delete won't be accessible in the working directory\
// it will be lost forever\
// important note about reset: can be use if the specific commit hasn't been pulled already by other dev, if it's the case DON'T use RESET instead use REVERT.\

`git revert <commit-hash>` // git revert will create a new commit which revers/undo the changes form a commit

# GIT REMOTE

`git remote -v` // list all remote connected repo\
`git remote add origin <repo url>` // it will connect the remote origin (origin git@github.com:harveynichols/web-local.git (fetch)) to the local repo

`git push origin master` // it will push all local change on master to the master remote branch\
`git push origin cats:master` // it will push all local change from the `cats` branch to the master remote branch.

`git push -u origin master` // it will connect the local branch master to the remote master branch\
// next time we can just run `git push` as branches are now connected.\
`git push --set-upstream origin dogs` // similar as above

# GIT PULL & GIT FETCH

origin/master // This is a `Remote tracking branch`, it's a reference to the state of the master branch on the remote.\
// I can't move this myself. It's like bookmark pointing to the last known commit on the master branch on origin.

`git branch -r` // view the remote branches our local repo know about\
`git checkout origin/master` // we can see what was the state of our files when we last make a pull.\
`git switch -` // after you detached the HEAD to origin/master point HEAD to the last commit of the branch.

When cloning a new repo we only get the default branch selected on github. For example if we have a github repo with default : master and another tools branch.\
Locally if we run git branch we will get : master.\
We need to see all the branches available by running `git branch -r`:\
HEAD/master\
origin/tools

`git switch tools` // it will create a local branch tools and link it to the origin/tools.\
`Branch "tools"` set up to track remote branch "tools" from origin.

`git fetch` // download the changes from origin to the local repo or update the `remote tracking branch` with the latest changes from the remote repo.\
`git pull` // download the changes in your local working directory

`git fetch origin master` // fetch only the data from the origin master\
=> a dev add a new commit and push it to origin master\
=> run git fetch origin to get the latest change from master.\
Doing a git fetch it will download the latest commmit pushed by the dev and the local branch origin/master is detached from the local branch.\
You can checkout origin/master to see the change.\
After the fetch git will tell you that your master branch is behind of origin/master from 1 commit.\

`git pull` // `git fetch` + `git merge`

# Project contribution

1. Fork the project on your github account
2. clone the fork locally
3. add upstream to the remote
4. do some work
5. push to origin
6. open a PR

`git remote add upstream https://github.com/Colt/fork-and-clone.git` // needed to pull down all the change from the repo owner
`git remote add origin https://github.com/stephaneHN/fork-and-clone.git` // needed to push commit to origin.

# GIT REBASE

`git rebase master` will rewrite the history on the branch where it is running e.g. feature branch\
all merged commit will be removed and and all commit from feature branch will appear on top of the latest commit from master branch.

`git switch feature` // switch to the branch where you want to rebase\
`git rebase master` // rebasing the feature branch with master.

:warning: never rebase commits that have been shared with others. If those commits are already on master branch or in\
others dev history DO NOT REBASE !

`git rebase -i HEAD~9` // it will open an editor with the selected commit and we can choose an option to make some change.
p, pick <commit> = use commit
r, reword <commit> = use commit, but edit the commit message
e, edit <commit> = use commit, but stop for amending
s, squash <commit> = use commit, but meld into previous commit
f, fixup <commit> = use commit, like 'squash', but discard this commit's log
x, exec <commit> = run command ( the rest of the line ) using shell
b, break <commit> = stop here ( continue rebase later with `git rebase --continue`)

# GIT TAGS

tags is used to annoted a specific commit and will stick with this commit.
It is commonly used to mark new release

2 types of tags:

1. **lightweight tags** are leightweight. They are just a name/label that points to a particular commit.\
2. **annotated tags** store some extra data including the author's name and email, the date, and a tagging message (like a commit message ).

## Semantic versionning

1 . 2 . 0
Major version . Minor verstion . patch

`git tag`
`git tag -l "v17*"` // list all the tag that start with v17\
`git log --oneline` // if a commit has a tag git log will also show the tag\
`git tag -l`
v17.0.0
v16.4.3
v16.4.2
`git checkout v17.0.0` // we can checkout the commit where that tag is v17.0.0\
`git diff v17.0.0..v16.4.3` // we can get the difference between 2 tags\

`git tag <tagname>` // create a leightweight tag, by default, git will create the tag referring to the commit that HEAD is referencing.\

`git tag -a v17.0.3` // create an annotated tag. git will open your default editor and prompt you for additional information.\
`git tag -am v17.0.0 "details about the tag which is an annotated tag"`

`git show v17.0.0` // this command will display all the details about the tag whithin the commit is refers to.\
`git tag <tagname> <commit>` // if HEAD doesn't point at the commit you want to create the tag we can specify the commit instead.

`git tag v17.0.0 696e736be -f` // use -f to force to move a tag on commit A to go on commit B. TAGS ARE UNIQUE.\
`git tag -d v17.0.0` // remove this tags

`git push origin v17.0.0` // push the tags v17.0.0 to origin\
`git push origin --tags` // push all tags created locally to origin.

# Git behind the scene

## Config folder

In any repo go to .git folder and ls -al

```
COMMIT_EDITMSG  HEAD            branches        description     index           logs            packed-refs
FETCH_HEAD      ORIG_HEAD       config          hooks           info            objects         refs
```

config file can setup property for a specific repo.
`git config --local user.name ="James Bond"` // it will setup this username only for this repo\
`git config user.name="James bond"` // whearas this cmd will setup this username globaly

`git config --local --list` // display local config properties

## Refs folder

Inside of refs, you'll find a heads directory. `refs/heads` contains one file per branch in repository.\
Each file is named after a branch and contains the hash of the commit at the tip of the branch.\

For ex. `refs/heads/master` contains the commit hash of the last commit on the master branch.

Refs also contains a `refs/tags` folder which contains one file for each tag in the repo.

In any repo go to .git/refs folder and ls -al

```
heads   remotes tags
```

`ls -al .git/refs/heads` // will retrieve all the branch where HEAD is pointing to.\

## Chryptographic hash function

git create hash with hexa-decimal notation with sha-1 which will always output 40 digit long\
If we run sha-1 function on the same character or the same file we will always get the same hash,\
hash function is call deterministic, same input yields same output.

Let's try hash-object !
`git hash-object <file>`

`git hash-object my_todos.txt` // it will create the hash keys\
`be856298709274fb57001a998ed0b38cfc32aec8`

`echo "Stephane Candelas" | git hash-object --stdin` // it will hash the output of the echo\
`0a8b3c9d66b1437a6eadad181cdaf2c9d6d2ca5d`

`echo "Stephane Candelas" | git hash-object --stdin -w` // the -w will store the generated hash in .git/objects/0a\
`0a8b3c9d66b1437a6eadad181cdaf2c9d6d2ca5d` // Oa is the hash prefix and also the folder name in `.git/objects

## git cat-file hash

`git cat-file -p <hash>` // will pretty print out the content of the hash

`blobs` : Git blobs ( binary large object ) are the object that git uses to store the contents of files in\
a given repository. Blobs don't even include the filenames of each fil or any other data,\
it just store the content of the file.

`tree` : Trees are Git objects used to store the contents of a directory. Each tree contains pointers than can\
refer to blobs and to other trees.

`git cat-file -p master^{tree}` // it will print out the tree structure of the current repo

```
100644 blob 83c831f0b085c70509b1fbb0a0131a9a32e691ac    README.md
100644 blob 6c3e55a8b08aa12d39854d4d4959a1048799b3f0    favorites.txt
100644 blob 8f6596a12441823aa5ad35cd1d5d3c9c951d20e0    hobbies.txt
100644 blob be856298709274fb57001a998ed0b38cfc32aec8    my_todos.txt
040000 tree a6fa942245a1e8dc119c27a59c83b678d502583f    test2
100644 blob abfc3688804d74ae581d1e25d5dfc6e26d89a160    todo.txt
```

`commit` : Commit objects combine a tree object along with information about the context that led to the current tree.\
Commits store a reference to parent commit(s), the author, the commiter, and of course the commit message.

# Git reflogs

`ls .git/logs/HEAD` // will retrieve all logs for all git command, checkout,commit, pull, merge, push etc ...

git only keeps reflogs on your local activity. They are not shared with collaborators.\
reflogs also expire. Git cleans out old entries after around 90 days, though this can be configured.

`git reflogs show master` // show the history of the moves
`git reflogs master@{2}` // show the history from master from 2 moves ago.

# Writing custom git Aliases

`cat ~/.gitconfig` // show the git config file
In .gitconfig it's possible to create aliases :

```
[aliases]
    s = status
```

Adding aliases with the command line:

`git config --global alias.showmebranches = branch` // add alias in the .gitconfig file.
