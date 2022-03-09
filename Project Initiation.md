# Project Initiation
This setup is in regards to using either Windows or Ubuntu (on Windows with WSL) development systems.

# Dependencies
- Before starting, verify that node, npm, SASS, and gulp-cli are globally installed. If unsure whether a package is installed, run 
```
npm i --g package-name
```
For simple non-React sites, Gulp may be useful as a task runner. Gulp dependencies include the following:
```
npm i gulp gulp-sass browser-sync gulp-sourcemaps gulp-cssnano gulp-imagemin gulp-cache gulp-htmlmin del gulp-autoprefixer gulp-babel @babel/core @babel/preset-env --save-dev
```
Composer will be needed for Wordpress sites. Note though that composer is already installed for the local XAMPP environment. To confirm installation, type `composer` into CLI. See [here](https://thecodedeveloper.com/install-composer-windows-xampp/) for how to install.

## jQuery an Waypoints
In this section you will find important information as to how jQuery interacts with other libraries when it is a dependency
- Bootstrap 4 and Waypoints use jQuery as a dependency, therefore jQuery must be independently installed via npm
- These libraries will access jQuery, but you cannot confirm that jQuery has been loaded on the page with the method below. `console.log(typeof $)` will return `undefined` even though jQuery is supporting these libraries as a dependency
- If only using as a dependency to libraries imported from node, jQuery does not have to be called in your `index.js` file. If you need to use jQuery directly in the file, add `import 'jquery';` to the top of the file
- You can confirm whether jQuery has been loaded on browser by adding `console.log(typeof $);` to your JS file. See above for stipulations.
- 	`undefined` means jQuery has not been loaded
- 	`function` means jQuery has been loaded
- Find all npm packages installed in a project with `npm list --depth=0`. This will show top level dependencies. Change the depth number to see lower level dependencies.

# Project Initiation for Ubuntu

## Setting up SSH for Github via [Ubuntu](https://ubuntu.com/tutorials/ssh-keygen-on-windows#1-overview)
Instructions for connecting to Github below require setting up public and private keys for SSH connection. This only has to be done once, unless you want to set up unique SSH keys for different projects. You can also generate keys via Windows powershell, but this is not necessary if following the instructions below.

**Generate a new key**
1. Open Ubuntu terminal
2. Verify ssh is installed: `sudo apt install openssh-client`
3. Generate a key: `ssh-keygen -t rsa`
4. You can see the file at `~/.ssh/`

SSH allows you to connect your Github account to remote servers and services, without supplying your username and personal access token. You must add a key to your account prior to using SSH to authenticate. `ssh-agent` is a key manager for SSH. It holds your keys and certificates in memory, unencrypted, and ready for use by `ssh`.

**Add the key to your Github account**
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

## Set up local and remote git repositories
- Log in to Github, click "New"
- In your main projects directory (/projects):
  ```
  echo > <project-name>
  cd /<project-name>
  git init
  touch README.md
  git commit -m "first commit"
  git branch -M main
  git remote add origin git@github.com:justjohnd/<project-name>.git
  git push -u origin main
  npm init -y
  npm install --save-dev --save-exact prettier
  echo > .gitignore
  echo > .prettierignore
  echo "node_modules" | tee .prettierignore .gitignore
  echo {} > .prettierrc.json
  ```

# Project Initiation for Windows
Before initiating a new project, confirm you have SSH set up. This process uses the PuTTy application in Windows. Instructions below are for using an Ubuntu environment. 

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
