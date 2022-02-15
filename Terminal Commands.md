# Linux subsystem on WSL2
Currently I am using two environments: Windows 10 and a Linux subsystem on WSL2. The latter is preferred and should be used for future projects, as for one, it makes Rails much easier to work with, and is more of an industry standard than Windows.

Ubuntu will open in `/home/justjohnd`

Unless otherwise noted, all commands can be executed from within Ubuntu.

## Commands regarding your linux distribution
- Check to see what version of WSL you are running: From Windows start, search for **Command Prompt**, right click and run as Administrator. Then enter `wsl --list --verbose`
- Check the current versuion of Ubuntu: `lsb_release -dc`
- Update your distribution: (This should be done regularly, and manually, as Windows does not provide automatic updates) `sudo apt update && sudo apt upgrade`

### Change environment variables systemwide (note: this doesn't currently work for me!)
- Enter environment file: `sudo -H vi /etc/environment`
- Add your value: `MONGOSH = 'mongosh "mongodb+srv://cluster0.r33ym.mongodb.net/myFirstDatabase" --username justjohnd'`
- Logout and login. Test `echo $MONGOSH`. Get nothing returned


### (Installing nvm)[https://docs.microsoft.com/en-us/windows/dev-environment/javascript/nodejs-on-wsl]
Node version manager is useful when operating in different environments such as Windows and Ubuntu. Prior to installation, however, node and npm should be uninstalled from the distribution and reinstalled after nvm is installed.
- Uninstall node, npm, and dependencies: `sudo apt-get purge --auto-remove nodejs`
- Install cURL: `sudo apt-get install curl`
- Install nvm: `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash`
- Close terminal and then verify installation: `command -v nvm`. This should return `nvm`
- List versions of node currently installed: `nvm ls`
- **Install current stable version of node**: `nvm install --lts`

## Find out current user:
`whoami`

## Change owner of file to current user:
`sudo chown -R $USER /file-path`

## File and Directory Manipulation
### Create File
Use the `touch` command, followed by the path, filename, and extension

### Change file or directory name:
`mv full/file/path/old-name full/file/path/new-name`
Ex.: `mv /home/user/temp /home/user/directory`

### Move file or directory:
**WARNING** When moving adirectory, make sure to specify the directory name at the end of the file path or it will overwrite the destination directory!
Ex.: `sudo mv restaurant /projects/restaurant`

If in the above example, you used a desination path `/projects`, the directory `restaurant` would be renamed `projects` and moved to the root (`/`) directory.

### Remove a directory and contents
`rm -r <directory name>`

## Learn how to find a file/directory (here)[https://devconnected.com/how-to-rename-a-directory-on-linux/]

# npm Commands
## Find all packages installed:
`npm list --depth=0`

# Misc:
**Generate random string of bytes**
This is good for creating JWT secrets.
```
node
require("crypto").randomBytes(35).toString("hex");
```




