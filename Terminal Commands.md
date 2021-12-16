# Linux subsystem on WSL2
Currently I am using two environments: Windows 10 and a Linux subsystem on WSL2. The latter is preferred and should be used for future projects, as for one, it makes Rails much easier to work with, and is more of an industry standard than Windows.

Ubuntu will open in `/home/justjohnd`

## Find out current user:
`whoami`

## Change owner of file to current user:
`sudo chown -R $USER /file-path`

## Change name of file/directory OR move file/directory:
### Change name:
`mv full/file/path/old-name full/file/path/new-name`
Ex.: `mv /home/user/temp /home/user/directory`

### Move directory:
**WARNING** When moving adirectory, make sure to specify the directory name at the end of the file path or it will overwrite the destination directory!
Ex.: `sudo mv restaurant /projects/restaurant`

If in the above example, you used a desination path `/projects`, the directory `restaurant` would be renamed `projects` and moved to the root (`/`) directory.

## Learn how to find a file/directory (here)[https://devconnected.com/how-to-rename-a-directory-on-linux/]

# npm Commands
## Find all packages installed:
`npm list --depth=0`



