# Project Initiation
This setup is in regards to using either Windows or Ubuntu (on Windows with WSL) development systems. Ubuntu is perhaps preferrable. 

# Setting up SSH
Instructions for connecting to Github below require setting up public and private keys for SSH connection. This is a similar procedure as used to connect to a Digital Ocean server, though that process uses the PuTTy application in Windows. Instructions below are for using an Ubuntu environment. This only has to be done once, unless you want to set up unique SSH keys for different projects. You can also generate keys via Windows powershell, but this is not necessary if following the instructions below.

## Setting up via [Ubuntu](https://ubuntu.com/tutorials/ssh-keygen-on-windows#1-overview)
1. Open Ubuntu terminal
2. Verify ssh is installed: `sudo apt install openssh-client`
3. Generate a key: `ssh-keygen -t rsa`
4. You can see the file at `~/.ssh/`

## Initiate git, created Github repository, and make remote connection. See Git section for more information

## Additional Dependencies
- Before starting, verify that node, npm, SASS, and gulp-cli are globally installed. If unsure whether a package is installed, run 
```
npm i --g package-name
```
- Gulp dependencies include the following:
```
npm i gulp gulp-sass browser-sync gulp-sourcemaps gulp-cssnano gulp-imagemin gulp-cache gulp-htmlmin del gulp-autoprefixer gulp-babel @babel/core @babel/preset-env --save-dev
```
- Bootstrap 4
- Composer. Note that composer is already installed for the local XAMPP environment. To confirm installation, type `composer` into CLI. See [here](https://thecodedeveloper.com/install-composer-windows-xampp/) for how to install.

### jQuery ###
In this section you will find important information as to how jQuery interacts with other libraries when it is a dependency
- Bootstrap 4 and Waypoints use jQuery as a dependency, therefore jQuery must be independently installed via npm
- These libraries will access jQuery, but you cannot confirm that jQuery has been loaded on the page with the method below. `console.log(typeof $)` will return `undefined` even though jQuery is supporting these libraries as a dependency
- If only using as a dependency to libraries imported from node, jQuery does not have to be called in your `index.js` file. If you need to use jQuery directly in the file, add `import 'jquery';` to the top of the file
- You can confirm whether jQuery has been loaded on browser by adding `console.log(typeof $);` to your JS file. See above for stipulations.
- 	`undefined` means jQuery has not been loaded
- 	`function` means jQuery has been loaded
- Find all npm packages installed in a project with `npm list --depth=0`. This will show top level dependencies. Change the depth number to see lower level dependencies.

## Download Wordpress and set up MySQL (for Bootstrap 4)
- Open the XAMPP Control Panel and start MySQL.
- Click Admin to open phpMyAdmin.
- Click New and enter database name.
- Download newest version of Wordpress from here: `https://wordpress.org/download/`, directly into the new project directory.
- Inside the directory, change the filename `wp-config-sample.php` to `wp-config.php`.
- Open `wp-config.php` and change:
-  `'database_name_here'` to your database name.
-  `'username_here'` to `'root'`
-  `'password_here'` to `''`
- In browser, open `http://localhost/my-project-name`.
- Enter username, password, and email information and click Create. For consistancy, use the name `admin_project-name`
- Copy your theme (ex: `bootstrap-theme`) into the themes directory
- Delete the .git directory from the theme
-In your gulpfile change the brwosersync proxy to your appropriate database path. Ex.: `proxy: 'http://localhost/database-name',`

## Configure Wordpress Dashboard
- Install Advanced Custom Fields plugin
- Create a field group with image arrays `image-1` to `image-8`
- Add default images to each field, and make the field group applicable to all posts and pages
- Activate the theme
- Create a blank Home Page called "Home"
- Under Customize, set the Homepage to "Static" and select the Home page
- Under Users, disable viewing of the toolbar on the site
- Check the site in the browswer; it should be loaded

