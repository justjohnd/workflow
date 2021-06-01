# Workflow

This document describes my worflow in detail for creating a Wordpress project (starting with a static site) for myself or a prospective customer

# Initial Setup
## Setting up git local and remote repos
- Click "Import repository"
- Under "Your old repository’s clone URL" enter <code>https://github.com/justjohnd/static-site.git</code>
- Name the repository and click "Begin Import"
- Open your new repository and under "Code" copy the HTTPS address
- In CLI (within your projects folder, for instance `/webDevelopment`) type: `git clone my-repository-path`
- Make a change to README (or any file), save and make your initial commit:
```
git add -A
git commit -m "first commit"
git branch -M main
git push origin main
```
- In CLI type <code>npm install</code> 

### Dependencies (Static Site)
- Node and npm (globally)
- SASS (globally): <code>npm install sass -g </code>
- npx: <code>npm i npx</code>
- In case you don't directly install dependencies from the cloned `static-site` above, you can also run the following in CLI:
 ```
npm i gulp-cli -g
npm i gulp gulp-sass browser-sync gulp-sourcemaps gulp-cssnano gulp-imagemin gulp-cache gulp-htmlmin del gulp-autoprefixer gulp-babel @babel/core @babel/preset-env --save-dev
```
- Bootstrap: <code>npm i --save bootstrap</code>
- Composer. Note that composer is already installed for the local XAMPP environment. To confirm installation, type `composer` into CLI. See [here](https://thecodedeveloper.com/install-composer-windows-xampp/) for how to install.

**Dependency Troubleshooting Notes**
- You can confirm whether jQuery has been loaded on browser by adding <code>console.log($)</code> to you JS file.
- Find all npm packages installed in a project with `npm list --depth=0`. This will show top level dependencies. Change the depth number to see lower level dependencies.

# Wordpress Integration (for Bootstrap 4)

## Setting up MySQL and Wordpress Locally
- Open the XAMPP Control Panel and start MySQL.
- Click Admin to open phpMyAdmin.
- Click New and enter database name.
- In the `xampp/htdocs/` folder, create a directory with your project name (Note: best practice is to name the project and directory the same).
- Download newest version of Wordpress from here: `https://wordpress.org/download/`, directly into the new project directory.
- Inside the directory, change the filename `wp-config-sample.php` to `wp-config.php`.
- Open `wp-config.php` and change:
-  `'database_name_here'` to your database name.
-  `'username_here'` to `'root'`
-  `'password_here'` to `''`
- In browser, open `http://localhost/my-project-name`.
- Enter username, password, and email information and click Create.
- At `https://underscores.me/` click Advanced, check ***_sassify*** and add any theme information you wish.
- Download and unzip as a file in your projects `themes` directory.
- Verify that node, npm, SASS, and gulp-cli are globally installed. If unsure whether gulp-cli is installed, run `npm i --g gulp-cli`
- Run `npm init`
- Initialize git and github local and remote repositories per the instructions above.

## Template Parts
- Template parts, along with custom posts will typically access content via ACF using the `get_field` function. Please paste the following snippet at 
Currently I do not have an automation process set up, however, please add the following snippet to the top of any page directly running get_field() for ACF:
```
<?php
$post = $wp_query->get_queried_object();
$name = $post->post_name; //Retrieves post name (currently not be used).
 $template_path_array = explode('/', $_template_file);
 $template_filename = end($template_path_array);
 $template_name = substr($template_filename, 0, strlen($template_filename) - 4);
?>
```
This standardizes field names to: `page-name-template-name-description`. Note that to keep with the template parts namespaced `-` convention, all ACF field names should be also set up using `-` instead of `_`.

## Header and Footer
- Use the deafault headers and footers for any full-width pages.
- Use the `sidebar` versions to include a righthand sidebar.

## Navbar
- See these `https://github.com/wp-bootstrap/wp-bootstrap-navwalker` and `https://gist.github.com/cristovaov/6306f833faa07608a1fe` for references.
- Copy the code from `class-wp-bootstrap-navwalker`. Create a new file in your root directory with the same name and paste the code in.
- Add this code to `functions.php`
```
/**
 * Register Custom Navigation Walker
 */
function register_navwalker()
{
	require_once get_template_directory() . '/class-wp-bootstrap-navwalker.php';
}
add_action('after_setup_theme', 'register_navwalker');
```
- See above website or Bootstrap template for boilerplate HTML. Paste this html into your header page.
- Make sure `'theme_location'` is pointed to the correct menu, and that menu is set up on the Wordpress dashboard
- See the above gist or Bootstrap Template to see how to add other elements, such as buttons, into the menu. In the `wp_nav_menu` function, `'container'` should be set to `''`. Then you can wrap the menu and additional elements in a single container
- You can add classes to the `li` that wraps each menu item by going to `class-wp-bootstrap-navwalker`, finding, and editing this portion of the code:
```
$atts['href'] = !empty($item->url) ? $item->url : '#';
// For items in dropdowns use .dropdown-item instead of .nav-link.
// Add classes to nav link level here:
if ($depth > 0) {
	$atts['class'] = 'dropdown-item';
} else {
	$atts['class'] = 'nav-link text-white';
}
```
- Use a custom-logo to allow the user to upload their logo image:
`<?php get_template_part('template-parts/content', 'custom-logo'); ?>`

## entry-footer
`function bootstrap_theme_entry_footer() {...}` is located in `incl/template-tags.php`. Edit this function if you want to change the appearance of the `.entry-footer` class.

## Waypoints and DOM Manipulation
- **Waypoints** allows you to manipulate the DOM based on user scroll position. The starter template currently uses it to change the navbar header background from transparent to white
- Use `element` to define the element which will call the `handler` function when that element is scrolled to
- As a best practice, namespace these elements as `js-elementName` to signify that they are not style elements
- Use `offset` to change the scroll position for that element to be triggered. A positive number will offset the starting point up from the initial element
- Example:
```
const waypoint = new Waypoint({
  element: document.getElementById('js-navbar'),
  handler: function () {
    document.getElementById('nav').classList.add('bg-white');
    document.getElementById('nav').classList.remove('bg-transparent');

    let elements = document.getElementsByClassName('nav-link'); // get all elements
    for (let i = 0; i < elements.length; i++) {
      elements[i].classList.remove('text-white');
      elements[i].classList.add('text-dark');
    }
  },
  offset: 200,
});
```
This code will call the `handler` function when `js-navbar` is scrolled to.

## CSS Transitions
- All transitions files reside in `src/sass/transitions`. 
- **Scroll-Out** is used to trigger CSS transitions on data-scroll
- `data-scroll` must be added to the container for which you impose the transition on scoll

## Advanced Custom Fields
- At this time, there is no automated solution for ACF, so fields must be entered manually, and should be done so only after site design has been finalized


# Client Information

Here you will find basic information on how to set up and maintain the content of your site. Before making any siginificant changes, please make sure you have a save backup of the site in case you want to rever to the previous version.

There are many places in the Wordpress Dashboard where your content can be changed. Depending on the page/post, most of the visible page content can either be changed directly in native editor, Gutenberg, or in custom fields at the bottom of the edit page. Custom fields are sometimes referred to as ACF (advanced custom fields) in this document.

## Important Security Notes

Currently all plugins have auto-updates disabled. It is very important to updated plugins regularly and this can be done easily by going to Plugins -> Installed Plugins and toggling the plugins to "Enable Auto-Update".

Before doing so, however, make sure you have a site backup system in place. Updates can sometimes have unintended, site-breaking consequences so have a backup ready.

## Adding / Editing Footer Information

Footer information, such as the company address, link titles, or social icons links can be edited uniquely for any specific page, but this is ill-advised. When setting up or changing footer content, it is best to bulk edit all pages/posts at once. To do this:

1. Go go Plugins -> Installed Plugins and activate the ACF QuickEdit Fields plugin.
2. Go to Pages and select all pages to edit.
3. In the Footer section select any field would like to change and edit the information.
4. Click Update.
5. Repeat the above procedure for all Posts.
6. Got back to Plugins and Deactivate the ACF QuickEdit Fields plugin (please do not delete it).

## Set Up Your Site Description (Tagline)

Setting your site's tagline is essential to helping search engines better index and rank your site. Do this by going to Settings -> General -> Tagline. Enter your description in the Tagline field and save.

## Set Up Your Metadata

Metadata is information about a website, which helps search engines index and rank it, making your site easier to find on the web. This site uses AIOSEO, a plugin that helps manage that information. AIOSEO does not recognize content in custom fields (used throughout this site). Hence, the plugin shows numerous unnecessary warnings in the Basic SEO and Readibility sections of the page or post. Please disregard these warning.

Note that all pages and posts are set up to automatically generate appropriate metadata by default, so you do not need to set it up. You may however tailor any page/post name and meta description to your liking using the AIOSEO fields:

1. Go to any page or post and click Edit
2. If you do not see the AIOSEO tool bar, click the three dots in the top right corner and then select AIOSEO from the Plugins section
3. Click Edit Snippet. Here you can manually change the title and description for any page.

## Editing Content

- You can change any text or images throughout the site by using either customized fields accessable in each post and page, or directly in the Wordpress page editor, as in the case of the Homepage, Enquiry Information, Company Information, and tour post pages. Note that these are the ONLY pages which should be edited in the native Wordpress editor, Gutenberg. Doing so on other pages could cause issues with the page.
- All images and text in the page must be set for each language page separately.
- Access footer fields at the bottom of the home page.
- Access "OUR PARTNERS" section title and image fields from the bottom of the home page
- Note that Contact page has two types of forms, each in Japanese and English. One is for general enquiries, the other is for specific enquiries about reservations.
- You can edit text and images, but not the structure of the site (except for certain pages listed above, via the native Gutenberg page builder). Adding additional parts to pages (other than the Home page and Company Information page) via the Wordpress editor will likely break the page.
- Using the Preview feature inside page/post edit views will not always show changes correctly. It is better to view changes in a separate browser window, by looking at the actual, published path

Note that any images uploaded should be compressed and optimized for the web. Overly large images will slow your site down.

## Changing the Homepage Carousel Images

Got to Posts, and you will find posts Slide 1, Slide 2, and Slide 3. The featured image for each post appears as an image in the Homepage carousel. Simply change the featured image to change the carousel.

## Creating New Pages, Posts

For the site to be multilingual all pages or posts created need an original title (for the Japanese page) and a copy of the page/post with the same title, plus an "-en" extension at the end. For example "Contact" is the Japanese Contact page, "Contact-en" the English version of the page.

1. Go to Pages or Posts and click "Add New".
2. Enter the title for the Japanese page
3. Add a featured image
4. Choose a page template
5. Fill in any custom fields with text or images at the bottom
6. If adding images, be sure to fill in the alt text field as well
7. Confirm changes and publish. Open the page in a new browser tab and reconfirm changes
8. Repeat the above steps, but add "-en" to the end of the title
9. Add the tag "english" as well
10. If you want the page/post displayed as a menu item in the top navigational bar, add the item to both English and Japanese menus under Appearance -> Menus.

All English pages/posts must also include the tag, "english".

When adding new pages, please add the pages to both the Japanese and English custom menus.

## Deleting Pages, Posts

Note that deleting a page will remove it from the menus. It will need to be readded, once a new page is created.

## Changing the Featured Tour

Use the tag, "featured" to designate any tour you wish to feature on the home page. Be sure to tag both the Japanese and the English post.

## Adding the Featured Products Widget to the Homepage

Under Appearance -> Customize -> Widgets, click Add a Widget, select Featured Product. Then click Publish.

## Troubleshooting

1. If content has disappeared from the page, go back to the page or post edit screen and click Update.
2. If you receive a strange message from Google Translate on your page, some custom fields are not showing correctly, or the LANGUAGES link is broken, check to make sure you have the appropriate tag ('english' for English pages) assigned to the page

# Developer Information

## Accessing the Site Locally

- At the XAMPP Control Panel start up Apache and MySQL.
- Type in localhost/my-project-name into browser window
- To access the Wordpress admin panel, add /wp-admin to the above URL, then enter your name and password

## Server Side

### Setting up a new Droplet (Digital Ocean)

1. Log in and from dashboard go to Marketplace. Select Wordpress Create Droplet. Connect your domain and check the DNS records point to the new IP address. You should have A records for both my-site.com and www.my-site.com.
2. With puttyGen, generate a private key for SSH access.
3. Select your image, location, plan, and SSH key. Use your private key to set up SSH.
4. Enter droplet IP address into other browser window, and confirm the IP address is live.
5. Open a puTTY session, and enter your IP address. Make sure that there is a path to the private key, under SSH -> Auth. Make sure under Data that user is set to root. It make take 12 up to 12 hours to be visible. Clear cache and reload the site
6. Log back in to SSH to set up SSL for your domains with the following:
   <code>certbot --apache -d my-site.com -d www.my-site.com</code>

To add an SSH key first open puTTY Key Generator, set passphrase, and generate key. Note you will use this passphrase any time you also use the key.

### Set up SSL Certificate (Digital Ocean)

1. From dashboard, Account -> Security
2. Under Certificates for Load Balancers and Spaces, click Add Certificate.
3. Add your domain, select subdomains, and enter a name. Your certificate will generate.

### Log in to SSH

1. Open PuTTY, and open Host Name.
2. Enter password (note: password authentication is enabled for SSH).

## Using SASS

SASS is installed, so you need to make any css changes to the sass/style.scss file. To watch for changes on the CLI, use
<code>$ sass --watch sass/style.scss:style.css</code>


## Uploading to the Server

1. Open Filezilla.
2. At File -> Site Manager, select server and click Connect.
3. For Digital Ocean hosted sites, this is the path to theme files:
   <code>/var/www/html/wp-content/themes</code>
4. Copy the relevant theme files from local site to the remote files by selecting, dragging, and dropping them into the new directory. All theme files will have been transferred.
5. Open MySQL Workbench.

### Connecting to the server (Using mySQL Workbench)

- Choose Connection Method: Standard TCP/IP over SSH
- SSH Hostname: IP address
- SSH Username: root
- SSH Keyfile:

- First open PuTTYgen and load private key. Remove passphrase and under Conversions, click Export Open SSH Key
- Save file
- Under SSH Keyfile, enter the path to the above file

- MySQL Hostfile: 127.0.0.1
- MySQL Server Port: 3306
- Username: wordpress
- Password: Get this password from the .digitalocean_password file from the server. (Can download with FTP)

6. Select on Database -> Connect to Database.
7. Select database on server, under SCHEMAS.

8. Open site Wordpress Admin Panel.
9. Select Tools -> Migrate DB.
10. Under Migrate tab, confirm that Export File is selected, and boxes are checked to save the file to your computer, and compress using gzip.
11. In the Find field, enter your local directory preceded by two forward slashes: <code>//localhost/my-site</code>
12. In the Replace field, enter your domain name (note IP address will not work) preceded by two forward slashes:
    <code>//my-site.com</code>
13. Click Export.
14. Unzip the exported .sql file.

15. Back in MySQL Workbench, select Server -> Data Import.
16. Under the Import from Disk tab, toggle Import from Self-Contained File
17. Enter the path to the uncompressed .sql file
18. Under the Import Progress tab, click Import. Your database files will be transferred
19. Go to site and verify that it has been updated. Note that there may be a delay in the time for the pages show new data

## Connecting your (gmail) account to your domain [reference](https://admin.google.com/u/0/ac/signup/setup/v2/verify/mx)

To route your emails to gmail:

1. Go to your Digital Ocean project, click on the domain, then go to MX records
2. Delete in previous, unneeded records
3. Under HOST, enter **@**
4. Under MAIL PROVIDERS MAIL SERVER, enter **ASPMX.L.GOOGLE.COM**
5. Under Priority, enter **1**
6. Under TTL, enter **3600**
7. Click Create, and verify the following addresses and priorities are also added:
   **ALT1.ASPMX.L.GOOGLE.COM.** **5**
   **ALT2.ASPMX.L.GOOGLE.COM.** **5**
   **ALT3.ASPMX.L.GOOGLE.COM.** **10**
   **ALT4.ASPMX.L.GOOGLE.COM.** **10**
8. Click ACTIVATE GMAIL at the link provided above.

## Setting up SMTP (for gmail and using WP Mail SMTP plugin) [reference](https://wpmailsmtp.com/docs/how-to-set-up-the-gmail-mailer-in-wp-mail-smtp/#create-app)

1. Go to [Google Cloud Platform](https://console.cloud.google.com/cloud-resource-manager)
2. Create a project
3. [Register your project](https://console.cloud.google.com/flows/enableapi?apiid=gmail&pli=1)
4. Under Set Up Your Creditials, select the Gmail API, Web Server (if applicable), and User data (if applicable)
5. Click What Credentials Do I Need button
6. Select Internal (if applicable)
7. Enter App Name, User Support Email, Logo, and Developer Email
8. Leave Scopes section blank, save and continue
9. Under OAuth Client ID:
   - Enter any name you like for Client Name
   - Under Application Type, select Web Application
   - Under Authorized Javascript URI's enter URL (https://my-site.com)
   - Under Authorized Redirect URI's enter the URI from the WP Mail SMTP Setup page (ex: https://connect.wpmailsmtp.com/google/)
10. Click CREATE
11. Under Credentials section, obtain your Client ID and Client Secret, and enter them into the WP Mail SMTP Setup page
12. Send test email
13. Set up SPF (if not already): Go to your domain information on your server and add a TXT file. In the value, add <code>v=spf1 include:\_spf.google.com ~all</code>
14. Set up DKIM (if not already):

- Log in as a superadmin to your Google Account.
- Got to Apps -> Gmail -> Authenticate Email.
- Click GENERATE NEW RECORD.
- In domain DNS records on the server, add a new TXT record
- Add the DNS Host Name and Value given by Google.
- Back in Google dashboard, click AUTHENTICATE.
- I DKIM is working, Status will change to "Authenticating email"

15. Add a DMARC record to DNS.

- Create a new TXT record
- Under Value, enter <code>v=DMARC1; p=none; fo=1; rua=mailto:email-name@my-site.com</code>
- Add <code>\_dmarc</code> to the Name.

## Setting Up Contact Forms 7 and Captcha [reference](https://themeisle.com/blog/how-to-set-up-contact-form-7/#:~:text=The%20first%20step%20to%20setting,displayed%2C%20click%20Install%20%3E%20Activate)

1. Install Contact Forms 7 plugin
2. Install Advanced noCaptcha & invisible Captcha plugin
3. Under Contact, add any necessary forms
4. 

## Setting Up Captcha for Contact Forms 7

1. Confirm Contact Forms 7 plugin is installed.
2. Confirm Advanced noCaptcha & invisible Captcha plugin is installed.
3. Under settings in the Captcha plugin, set the version to V3
3. Set up reCAPTCHA at Google and assign account to local server (for development) and site. Domain should be set to `localhost`. Make sure that your version is also set to V3.
4. Under Contact -> Integration, enter site key and secret key.
5. When creating contact forms:
- Just above the Submit line, enter the following code:
   <code>[anr_nocaptcha g-recaptcha-response]</code>
- Add any necessary classes directly in the form on the dashboard. For example,
```
<label> Name
    [text* your-name class:form-control placeholder "Enter your name here"] </label>
```
Applies the class "form-control" to the text field. This is useful in assigning bootstrap classes to the fields
- Adjust any additional styling in style.scss
- Copy the shortcode and save
- In the page php file enter the following into your form:
`    <?php echo do_shortcode(wp_kses_post('[shortcode]')) ?>
6. To remove recapcha badge, add the following to css (but make sure to include Googles privacy policy on the site):
```.grecaptcha-badge {
  visibility: hidden;
}
```

## Theme Structure Notes

- **single.php** is used for posts, which are also used for created Tours pages. single.php in turn uses template part **content.php**
  -Template part **content-featured** is only used to embed posts into the home page. Changes to this file will only be reflected on the home page, not within any of the post pages

  ## General Escaping Notes

  All HTML should be escaped to avoid hacks.

  - Custom fields: `<?php esc_attr(the_field('field-name')); ?>`
  - Urls: `<?php echo esc_url('url-name'); ?>`
  - Variables to print: `<?php echo esc_html($variable); ?>`
    or `<?php echo esc_attr($variable); ?>`
  - Short codes: `<?php echo do_shortcode(wp_kses_post('[short-code]')) ?>`

  ## Setting Metadata

  The plugin AIOSEO is installed, which allows you to manually change titles (if you wish to do so).

  Descriptions are populated automatically in header.php for any page/post using ACF. For this reason, the first block of text found in ACF should always be titled <code>intro_text</code>. If Gutenberg is being used for the page instead of ACF, you can set the description to Page Content.

  ## Using ACF

  - Changes to ACF structure are not always rendered in the built in Wordpress page preview. It is recommended you refresh the page in a separate browser window to view or confirm changes.
  - ACF are unique to each page/post. For example, you cannot set the image for the English version of a page inside the Japanese page. Each must be set separately.

## Using the Featured Products Widget

1. Add this anywhere to any page where you'd like the Featured Products widget to appear: <code><?php get_sidebar() ?></code>
2. Under Appearance -> Customize -> Widgets, click Add a Widget, then select Featured Products

## Git Cheatsheet
- **Remove untracked files**:
```
git stash save --keep-index --include-untracked
git stash drop
```
