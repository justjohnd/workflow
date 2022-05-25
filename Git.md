Git uses command and options. For instance `git add -A` uses the command `add` and the option `-A`.

Note that GitHub does not show empty directories.

# Common commands and their options
## `add`
`add` allows you to stage files
**Options**
- `.` will add all changes in the current directory only
- `-A` will add all changes in all directories

## `branch`
Tying `branch` alone will show all branches, as well as the current branch
**Options**
- `-M <new-branch-name>` will change the name of the current branch.
- `-M <old-branch-name> <new-branch-name>`

## `remote`
Controls access to a remote repository
- `add origin <path>` creates a new remote repository called `origin` at the path specified

## `git stash` to stash a file
Remove untracked files:
```
git stash save --keep-index --include-untracked
git stash drop
```

## `log`
See commit log for branch: `git log <branch-name>`

## `checkout` to temporarily check out previous commit
This is good when you want to go back, but be sure to maintain the integrity of HEAD
`git checkout <commit-hash>`

## `revert`
Note that it is not possible to revert commits directly from the GitHub site. You can, however, revert a commit on your local repo.
- From the command line, revert to a previous commit: `git revert <commit-hash>`
- You can also revert from the Command Pallette in VS Code by typing `git undo` and selecting "Last Commit"


## `rm`
`rm` can be used with the option `--cached` in order to remove files or directories that have been accidentally added to the remote repo
1. First add the files or directories to youre `.gitignore` file
2. In terminal, type `git rm --cached <filename>` or `git rm -rf --cached <directory>`. This will remove these items from the git staging index, but won't delete theme.
3. Commit all changes and push to remote.
**WARNING: When doing `git pull` it is possible that the remote repo will overwrite and remove the files. Make sure to have made copies of all the files beforehand! Because they are not longer being tracked by git the will otherwise be gone forever**

The `rm` command is also a regular terminal command (the `git` prefix is not needed), which can be used for the following:
- Remove (Undo) git init: `rm -rf .git`
- Remove directory and all contents
`rm -r <directory-name>`

# Project Initiation (with git and Github)

## Connecting to Github via SSH
Note, that this can typically be skippeds as SSH has already been set up for the account. Use for reference.
SSH allows you to connect your Github account to remote servers and services, without supplying your username and personal access token. You must add a key to your account prior to using SSH to authenticate. `ssh-agent` is a key manager for SSH. It holds your keys and certificates in memory, unencrypted, and ready for use by `ssh`.

1. First, ensure agent is running: `eval "$(ssh-agent -s)"`
2. [Add SSH key to SSH agent](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent). Add private key to ssh agent: `ssh-add ~/.ssh/id_rsa`. **Note:** do not enter the .pub extension here
3. Open your public ssh key:
  ```
  cd ~/.ssh
  cat id_rsa.pub`
  ```
4. Copy it to the clipboard
5. Go to your account on Github, and under SSH add the key
6. You will need to use the ssh path when setting up new repos
Note: See [here](https://docs.github.com/en/get-started/getting-started-with-git/managing-remote-repositories) to see how to change an HTTPS repo over to SSH

## Set up git local and Github remote repos
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

# Compare branch file with main file
In VS Code, while in the branch you want to compare with main, click the Source Control icon on the left. Then click SEARCH & COMPARE and click on the file you want to compare.

# .gitignore
Place a `.gitignore` file in your parent directory to prevent certain files from tracked. Note that this is for the local and remote directories!

.git ignore will also ignore subdirectories and files. For example: `themes/bad-themes` will ignore the sub-directory `bad-themes` and all of its files.
