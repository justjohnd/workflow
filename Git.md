#### Change master branch name to main
git branch -M master main

#### Compare branch file with main file
In VS Code, while in the branch you want to compare with main, click the Source Control icon on the left. Then click SEARCH & COMPARE and click on the file you want to compare.

### Stash a file
`git stash`

#### Remove untracked files
```
git stash save --keep-index --include-untracked
git stash drop
```

#### Remove (Undo) git init
`rm -rf .git`

#### Remove directory and all contents
`rm -r <directory-name>`

### See commit log for branch
`git log <branch-name>`

### Temporarily check out previous commit
This is good when you want to go back, but be sure to maintain the integrity of HEAD
`git checkout <commit-hash>`

### Revert to a previous commit
`git revert <commit-hash>`

