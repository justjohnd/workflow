# Compare branch file with main file
In VS Code, while in the branch you want to compare with main, click the Source Control icon on the left. Then click SEARCH & COMPARE and click on the file you want to compare.

# Remove untracked files
```
git stash save --keep-index --include-untracked
git stash drop
```
- Remove directory and all contents: `rm -r directory-name`
