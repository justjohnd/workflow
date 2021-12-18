# Project Initiation (with git and Github)
## Connecting to Github via SSH
1. [Add SSH key to SSH agent](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent). First, ensure agent is running: `eval "$(ssh-agent -s)"`
2. Add private key to ssh agent: `ssh-add ~/.ssh/id_rsa`. **Note:** do not enter the .pub extension here
3. Open your public ssh key (`id_rsa.pub`) and copy it to clipboard
4. Go to your account on Github, and under SSH add the key
5. You will need to use the ssh path when setting up new repos
Note: See [here](https://docs.github.com/en/get-started/getting-started-with-git/managing-remote-repositories) to see how to change an HTTPS repo over to SSH

# Set up git local and Github remote repos
- Log in to Github, click "New"
- In your main projects directory (/webdevelopment): `repo project-name`

This alias runs the following function script (for windows), which will set up a local repo, run npm init, set up Prettier, and set up ignore files. Note that the path is for a repo set up with SSH
```
repo () {
  mkdir $1
  cd $1
  echo "# $1" >> README.md
  git init
  git add -A
  git commit -m "first commit"
  git branch -M main
  git remote add origin git@github.com/justjohnd/$1.git
  git push origin main
  npm init -y
  npm install --save-dev --save-exact prettier
  echo > .gitignore
  echo > .prettierignore
  echo "node_modules" | tee .prettierignore .gitignore
  echo {}> .prettierrc.json
}
```

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

