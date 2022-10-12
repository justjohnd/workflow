# Introduction

This document describes my worflow in detail for creating a Wordpress or static site project, using Bootstrap 4 and gulp, as well as documentation intended for the administrator of that site.

Read about general [project initiation](https://github.com/justjohnd/workflow/blob/main/Project%20Initiation.md)

# Absolute Basics
This section assumes a local server has already been connected.

All WordPress themes must have, at minimum:
- functions.php
- index.php
- styles.css

* The appropriate way to load css and js is via enqueuing in the functions.php file.

## Custom Post Types
Custom post types are used to organize data that doesn't fall into the native WordPress post types, such as products, brands, etc.
* To create custom post types (example: mysite.com/brands/nike where brands is the custom post type), register the custom post type in functions.php (or create a plugin (see sandbox for example)
* To query for a post type use `post_type`

## Loops
* Use the Loop for querying posts, `WP_Query()` for other things, like custom post types.

# John's FAQ
## Retrieving data from database
Q: How do I retrieve and display all posts?
A: Use the `have_posts()` function and loop and use a 'while' loop to loop through it.

Q: How do I retrieve pages under a custom post type?
A: Use either the `WP_Query {}` query class or `get_posts()` function, and filter by the `post_type` property.

Q: How do I limit the number of posts shown?
A: Pass the `posts_per_page` parameter to your `WP_Query($args)` or `number_posts` to your `get_posts($args)`

## Enqueuing
Q: Is it less performant to call `wp_enqueue_scripts()` multiple times?
A: It doesn't **seem** to be less performant

Q: What's the difference between:
`get_stylesheet_uri()`
`get_stylesheet_directory()`
`get_template_directory_uri()`
A: 
`get_stylseheet_uri` will return the uri that includes `style.css` appended to it 
`get_stylesheet_directory()` will return the path to the directory for the **active** theme
`get_template_direcotyr_uri()` will alays return the path for the **parent** theme





# Opinionated Project Setup

## Download Wordpress and set up MySQL (for Bootstrap 4)
- Open the XAMPP Control Panel and start MySQL.
- Click Admin to open phpMyAdmin.
- Click New and enter database name.
- Download newest version of Wordpress from here: `https://wordpress.org/download/`, directly into the new project directory a
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

# Wordpress Front End

## General Escaping Notes
All HTML should be escaped to avoid hacks.
- Custom fields: `<?php esc_attr(the_field('field-name')); ?>`
- Urls: `<?php echo esc_url('url-name'); ?>`
- Variables to print: `<?php echo esc_html($variable); ?>`
  or `<?php echo esc_attr($variable); ?>`
- Short codes: `<?php echo do_shortcode(wp_kses_post('[short-code]')) ?>`

## Project Sections ##
### Posts ###
- **single.php** is used for posts by default, which are also used for created Tours pages. single.php in turn uses template part **content.php**
- Custom post templates can be set up by adding the following to `functions.php`. Note, however that this will disable `single.php` and all posts without a custom template as defined by their category will default to `index.php`.
```
<?php

// Create custom single post for category posts
/*
* Define a constant path to our single template folder
*/
define('SINGLE_PATH', TEMPLATEPATH . '/single');

/**
* Filter the single_template with our custom function
*/
add_filter('single_template', 'my_single_template');

/**
* Single template function which will choose our template
*/
function my_single_template($single)
{
	global $wp_query, $post;

	/**
	* Checks for single template by category
	* Check by category slug and ID
	*/
	foreach ((array)get_the_category() as $cat) :

if (file_exists(SINGLE_PATH . '/single-cat-' . $cat->slug . '.php')) {
	return SINGLE_PATH . '/single-cat-' . $cat->slug . '.php';
} elseif (file_exists(SINGLE_PATH . '/single-cat-' . $cat->term_id . '.php')) {
	return SINGLE_PATH . '/single-cat-' . $cat->term_id . '.php';
}

	endforeach;
}
```
- To get custom post templates to work you must also set up directory in root called `single`. Inside, store all custom post files with the following namespace: `single-cat-[category name].php`


### Template Parts ###
- Template parts, along with custom posts will typically access content via ACF using the `get_field` function. Please paste the following snippet at 
the top of any page directly running get_field() for ACF:
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

### Header and Footer
- Use the deafault headers and footers for any full-width pages.
- Use the `sidebar` versions to include a righthand sidebar.
- For any project, Wordpress or otherwise, use the following link for accessing the Font Awesome CDN: **https://use.fontawesome.com/releases/v5.13.1/css/all.css**

### Navbar
#### Navwalker
- See [here](https://github.com/wp-bootstrap/wp-bootstrap-navwalker) and [here](https://gist.github.com/cristovaov/6306f833faa07608a1fe) for references on how the navwalker file is used. Navwalker is necessary for bootstrap to work with the Wordpress Navbar.
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
#### Custom Logo
- Use a custom logo to allow the user to upload their logo image:
`<?php get_template_part('template-parts/content', 'custom-logo'); ?>`
- Change logo styles by .custom-logo class

#### General
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
#### Main and Custom Navbars (Developers Notes)
Note that the main navbar was actually found to have some bugs, especially on ipad. This navbar uses the waypoints.js library to trigger various DOM elements at a set scroll point. The result is that when the user scrolls past the banner carousel on the front page, navbar transitions from a transparent background to a solid background. Menu item colors and logo also changes for visibility.

Because this navbar is not ready for production, the site is set up with header('sidebar'). This is a simple sticky navbar with a dark background, used on all pages other than the front page. It does not need to be used with footer('sidebar').

### entry-footer
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

## Using SASS
SASS is installed, so you need to make any css changes to the sass/style.scss file. To watch for changes on the CLI, use
<code>$ sass --watch sass/style.scss:style.css</code>

## CSS Transitions
- All transitions files reside in `src/sass/transitions`. 
- **Scroll-Out** is used to trigger CSS transitions on data-scroll
- `data-scroll` must be added to the container for which you impose the transition on scoll

  ## Setting Metadata
  - The plugin AIOSEO is installed, which allows you to manually change titles (if you wish to do so).
  - Descriptions are populated automatically in header.php for any page/post using ACF. For this reason, the first block of text found in ACF should always be titled <code>intro_text</code>. If Gutenberg is being used for the page instead of ACF, you can set the description to Page Content.

  ## Using ACF
  - Changes to ACF structure are not always rendered in the built in Wordpress page preview. It is recommended you refresh the page in a separate browser window to view or confirm changes.
  - ACF are unique to each page/post. For example, you cannot set the image for the English version of a page inside the Japanese page. Each must be set separately.

## Using the Featured Products Widget
1. Add this anywhere to any page where you'd like the Featured Products widget to appear: <code><?php get_sidebar() ?></code>
2. Under Appearance -> Customize -> Widgets, click Add a Widget, then select Featured Products

## WMPL ##
- To be able to translate strings in the theme via WPML the text must be wrapped in a gettext call. Example:
```<a href="#"><?php _e('******', 'text-domain'); ?></a>```
where '******' is the string string you wish to translate and 'text-domain' is your theme name (e.g.: 'bootstrap-theme').
- For the home link to work, you also need to replace
```<?php echo esc_url(site_url('/')); ?>"```
with
```<?php echo apply_filters('wpml_home_url', get_option('home')); ?>```
for the href path.

# Wordpress Backend

## Accessing the Site Locally
- At the XAMPP Control Panel start up Apache and MySQL.
- Type in localhost/my-project-name into browser window
- To access the Wordpress admin panel, add /wp-admin to the above URL, then enter your name and password

## Setting up and accessing the server (Digital Ocean)
1. Log in and from dashboard go to Droplets -> Create
- Click on the Marketplace tab and select Wordpress. 
- Choose the number of droplets you wish, and name them.
- If you already have an SSH key created, select it for use. Otherwise create a new SSH key. You will use this private key to set up your SSH portal (if it hasn't been set up already). Use puttyGen to generate a private key for SSH access.
- Note that the public key in in Digital Ocean must match the one that was generated using puTTyGen. If unsure, open puTTyGen and load your public key. Check it against the public key attached to the account. **Changing keys assigned to your droplet after the droplet is created will lock you out of the Console**
- Create droplets
2. If you want to connect a domain, make sure the DNS records point to the new droplet IP address. You should have A records for both my-site.com and www.my-site.com.
3. Enter droplet IP address into other browser window, and confirm the IP address is live. Make sure you are opening the correct IP address. 
5. Load a puTTy session, but before doing so, confirm your IP address is correct. Make sure that there is a path to the private key, under SSH -> Auth. Make sure under Data that user is set to root. 
7. Once logged in to the SSH, continue with Wordpress configuration. Enter domain (ex. `my-site.com`), email, username, password, site title.
8. Set up LetsEncrypt for get SSL for site.
9. It make take 12 up to 12 hours for the site to be visible. Clear cache and reload the site

### Creating a Public Key
Do this if you want to access SSH directly from your local computer
1. In CMD type `ssh-keygen`
2. Enter through the prompts, setting the location to `~/.ssh/id_rsa.pub` (default file name and path)
3. Navigate to the directory (note that it will be a hidden file in `/Users`)
4. View file by `cat id_rsa.pub`
5. Copy the public key, add a new key in the DO control panel, under the Security section
6. Paste the key into the key field, name and save
7. Be sure to add this key to your Droplet when creating it
8. To access SSH via cmd, type `ssh root@ip-address`

**Additional Notes**
- SSL can also be set up manually with the following: <code>certbot --apache -d my-site.com -d www.my-site.com</code>
- To add an SSH key first open puTTY Key Generator, set passphrase, and generate key. Note you will use this passphrase any time you also use the key.

### Set up SSL Certificate (from Digital Ocean Dashboard)
1. From dashboard, Account -> Security
2. Under Certificates for Load Balancers and Spaces, click Add Certificate.
3. Add your domain, select subdomains, and enter a name. Your certificate will generate.

### Creating SSH on Windows machine
DigitalOcean droplets are Linux based. If you want to generate an SSH, you need to use PuTTYgen, which creates SSH keys, as well as PuTTy the utility used to connect your machine to the server via SSH
1. Open PuTTygen and either generate a new key or load one (if you've created one in DigitalOcean)
2. Select **Save Private Key**

### Creating Users in SSH
1. Log in to SSH
2.Type `ssh root@your-swerver-ip`
3. Click Yes to continue
4. Type `adduser user-name`
5. Enter any information you wish
- To create a superuser, type `usermod -aG sudo user-name'
- To change users, type `su johnd`

### Creating Virtual Hosts
Virtual hosts could be used for creating staging sites, or hosting multiple sites on a single droplet. Before following the tutorial below, confirm that you have created a non-root user with superuser privilidges. Then log in to putty with your key password, and change the user from root: `su johnd`
Refer to this tutorial for setting up a virtual host: https://www.digitalocean.com/community/tutorials/how-to-set-up-apache-virtual-hosts-on-ubuntu-18-04

### Creating a Subdomain
- To creat a subdomain, on the Digital Ocean dashboard go to Networking -> Domains
- Enter in your host name (Ex.: dev.my-site.com)
- Create an A record
- Check the URL in the browser. It will redirect to the site domain until is it point to another virtual host.

## Uploading Files via FTP to the Server
1. Open Filezilla.
2. If connecting for the first time, set up your connection under Site Manager, setting the Protocol to "SFTP - SSH File Transfer Protocol". Enter your IP addres in the Host field.
3. If you have connected in the paset, got to File -> Site Manager, select server and click Connect.
4. For Digital Ocean hosted sites, this is the path to theme files:
   <code>/var/www/html/wp-content/themes</code>
4. Copy the relevant theme files from local site to the remote files by selecting, dragging, and dropping them into the new directory. All theme files will have been transferred.
5. Open MySQL Workbench.

### Uploading mySQL database to the Server (Using mySQL Workbench)
- Choose Connection Method: Standard TCP/IP over SSH
- SSH Hostname: IP address
- SSH Username: root
- SSH Keyfile (note that the key should already exist under the name `exportedKey`:
	* First open PuTTYgen and load private key. Under Conversions, click Export Open SSH Key
	* Save file
	* Under SSH Keyfile, enter the path to the above file

- MySQL Hostfile: 127.0.0.1
- MySQL Server Port: 3306
- Username: wordpress
- Password: Get this password from the .digitalocean_password file from the server, under the `root` directory. It can be downloaded from FTP. Use the `wordpress_mysql_pass`

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

# Setting up Email
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

## Setting Up Contact Form 7 and Captcha [reference](https://themeisle.com/blog/how-to-set-up-contact-form-7/#:~:text=The%20first%20step%20to%20setting,displayed%2C%20click%20Install%20%3E%20Activate)
1. Install Contact Forms 7 plugin
2. Install Advanced noCaptcha & invisible Captcha plugin
3. Under settings in the Captcha plugin, set the version to V3
4. Set up reCAPTCHA at Google and assign account to local server (for development) and site. Domain should be set to `localhost`. Make sure that your version is also set to V3.
5.  Under Contact -> Integration, enter site key and secret key.
6. Under Contact, add any necessary forms
7. When creating contact forms:
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
    This site is protected by reCAPTCHA and the Google
    <a href="https://policies.google.com/privacy">Privacy Policy</a> and
    <a href="https://policies.google.com/terms">Terms of Service</a> apply.
8. To remove recapcha badge, add the following to css (but make sure to include Googles privacy policy on the site):
```
.grecaptcha-badge {
  visibility: hidden;
}
```

# Client Information

Here you will find basic information on how to set up and maintain the content of your site. Before making any siginificant changes, please make sure you have a saved backup of the site in case anything goes wrong and you need to reinstate a previous version.

There are many places in the Wordpress Dashboard where your content can be changed. Depending on the page/post, most of the visible page content can either be changed directly in native editor--Gutenberg--or in custom fields at the bottom of the edit page. Custom fields are sometimes referred to as ACF (advanced custom fields) in this document.

## Accessing the Dashboard ##
- Go to my-site.com/wp-admin
- Login using your login/password credentials
- If you have any issues logging in, request a new password or contact your Administrator

## Important Security Notes

Currently all plugins have auto-updates disabled. It is very important to update plugins regularly and this can be done easily by going to Plugins -> Installed Plugins and toggling the plugins to "Enable Auto-Update". However, I recommend not using auto-updates, because there are some cases when updates can break your site. However, it is critically important to keep your plugins updated. You can read more about the pros and cons of enabling auto-updates [here](https://www.wordfence.com/blog/2020/08/wordpress-auto-updates-what-do-you-have-to-lose/).

Before doing so, however, make sure you have a site backup system in place. Updates can sometimes have unintended, site-breaking consequences so have a backup ready.

## Set Up Your Site Description (Tagline)

Setting your site's tagline is essential to helping search engines better index and rank your site. Do this by going to Settings -> General -> Tagline. Enter your description in the Tagline field and save.

## Set Up Your Metadata

Metadata is information about a website, which helps search engines index and rank it, making your site easier to find on the web. This site uses AIOSEO, a plugin that helps manage that information. AIOSEO does not recognize content in custom fields (used throughout this site). Hence, the plugin shows numerous unnecessary warnings in the Basic SEO and Readibility sections of the page or post. Please disregard these warnings.

Note that all pages and posts are set up to automatically generate appropriate metadata by default, so you do not need to set it up. You may however tailor any page/post name and meta description to your liking using the AIOSEO fields:

1. Go to any page or post and click Edit
2. If you do not see the AIOSEO tool bar, click the three dots in the top right corner and then select AIOSEO from the Plugins section
3. Click Edit Snippet. Here you can manually change the title and description for any page.

## Images
Large images can slow your site way down. Here are some tips for image optimization.
- Crop your images to the desired shape (square or rectangle). This can be done in Wordpress, but I recommend doing before hand before moving to media library. You can crop images [here](https://www.iloveimg.com/crop-image).
- Resize your images to the minimum pixel size require for the largest screen. To determine this size, on a Chrome browser, go to your website. Then right click -> Inspect. You should see a Developer Tools section open up over part of the page. Make sure that the tools are located across the bottom of the page so that the page itself is still full width. Then at the far left end of the Developer Tools toolbar, click the Element Selector icon. Now hover your mouse over the desired image. You will see the dimensions displayed. Take not of the width. This is what you will set your minimum width to for that image.

![image](https://user-images.githubusercontent.com/55176130/128441885-312970bc-9ab8-436c-afee-a66fd2cd9601.png)

You can use Wordpress to resize images, but I recommend doing so before moving to media library. [Here](https://bulkresizephotos.com/en) is an online tool you can use.
- Compress your images prior to using. [Here] is a good online tool.
- In Wordpress, always add alt text to your image. If using WMPL, also add a translation.

### SSR990 Theme Maximum Image Width Reference ###
- Full screen-width images: front page banner and DJ page Background Image: 1264px
- Front page DJS AND SHOWS and LATEST NEWS: 730px
- Front page UPLOAD MUSIC and RECENT POSTS: 364px
- Front page EVENTS, ABOUT and CHARTS: 350px
- DJ images: 250px
- Blog post Background Image or images added in block editor: 852px
- Sound Street logo: 250px
- Other front page logos:
	Play store: 250px
	Apple store: 220px
	Soundcloud and Mixcloud: 248px
	Patreon: 270px
	Paypal: 226px

## Categories
Categories are used to index posts based on the category applied to the post.

**SSR990 Theme Notes**
- This them is set up already with the following categories for posts: DJs, Blog, and News. DJ's category aggregates DJ bio pages. Blog category is to be used for "Recent Playlists". News is intended for blog posts related to SSR990 as an organization. All other categories are to be used for music genres and will be shown in the side bar on various other pages.

Note that at this time, creating a new catetory is possible, but that category name will also appear under the "Music Genres" section of the sidebar.

## Posts and Pages
Posts and pages here work just like in any typical wordpress setup. Pages are intended for static pages, such as your Contact page. The front page of the site is set to "static" as well, meaning new blog posts do not appear on it, and it will not change unless you change the content of the page.

Posts have two purposes in this theme: one is to create and show blog posts; the other is to house indivual informational pages for your authors. "Blog Posts" refers to any post other than an author page. Your theme has already been tailored so that blog posts will be displayed in various pages on the site based on their categories, for example "News," or "Recent Playlists", i.e.: the post category determines where you will see the post on the site. The methods for adding, editing, and deleting posts is the same for any post type, with any exceptions listed below. See more information in the categories section below.

## Add and Edit Blog Posts ##
- Go to Posts → Add New to create a new post page.
- Title the post
- Add any text or image content directly into the Wordpress editor
- (**SSRT Theme**) For DJ pages, under "DJ Image", add an image of the DJ. 
- Under Background Image, add an image. (**SSRT Theme**) This image will either be shown as a banner image at the top of the page for DJ pages, or be shown in the body of the post (at the top) for all other pages. This image will also be used as a large thumbnail image for the post.

In the side panel under Posts:
- Verify the Author matches the author name
- Under Categories, check the appropriate category. (**SSRT Theme**) For DJ pages, include the DJ category along with any music genres relevant to the DJ. For all other posts pick only one category suitable to the post, for example, Blog or News. Note that multiple categories can be selected and will appear on multiple pages, but for non-DJ pages this should be avoided.
- When satisfied with all changes, click Update, then Publish
- From the main Posts or Pages panel, add a translation per the instructions below in Translate Post or Page
- **Be sure to check your actual post online and make sure all images are showing on the translated page** If they are not, see the WPML Troubleshooting secition below.

## Deleting Pages, Posts
Note that deleting a page will remove it from the menus. It will need to be re-added, once a new page is created.

## Add and Edit Users
- Recommended user roles for the site are: Administrator and Author. All DJ’s may be assigned an Author role, allowing them to create, publish, and edit their own posts. (They may not edit other authors’ pages).
- The Profile Press plugin allows you to update user entries with a profile picture, which in turn is presented on the home page under the “OUR MEMBERS” section. If this plugin is disabled, the profile picture will not appear on the home page.
- Prior to adding a new user, please create a new Author page for the user per the instructions above.
- When creating a new Author user, paste the URL to their Author page into the Website field

## Edit Menu
- You should not have to change your site menu, but it is possible to add or remove pages from the navigation bar. Please note that this is limited to the desktop only, and not mobile.
- Changes to the Contact, Donate, Events, and More buttons, as well as any changes to the mobile menu must be made by a developer.
- You can add / remove pages from the desktop menu under Appearance → Menus. Menu name is Menu 1

## How to Use WPML
### Installation and Setup (Developer)
- Refer to ----- for general insallation instructions
- Under settings, click "Use WPML's Advanced Translation Editor"
- When using ACF be sure to set all images to "Copy" (not "Translate") under WMPL settings
- Other fields can be set to "Translate" or "Don't Translate". For example links would not be translated, but typically text fields would

### Translate Post or Page
- Go to WPML -> Translation Management. 
- Select Posts or Pages you wish to translate. Note that you can send multiple posts/pages at the same time.
- Click ***Add Selected Content to Translation Basket*** button at bottom of page.
![image](https://user-images.githubusercontent.com/55176130/126728839-48acde91-b814-45b0-9ced-84f91ed18343.png)
- Click on the Translation Basket tab.
- Under number 3, "Choose translator or Translation Service" choose the name of the person you wish to assign translation duties to.
- Click ***Send all items for translation*** at the bottom of the screen
![image](https://user-images.githubusercontent.com/55176130/126729060-93af5c53-f99c-4678-a42e-e0117dfdad1c.png)
- Click WMPL -> Translations and click the ***Translate*** button for the page/post you wish to translate
![image](https://user-images.githubusercontent.com/55176130/126729175-6bcb7be0-28d3-407b-9185-c56a2c55d3f3.png)
- You will now be on the Advanced Translation Editor page. Note that original-language text may appear in individual boxes, depending on whether text was entered into the Wordpress editor in separate blocks or not. To combine sections (for instance, if you adding a large transalation for multiple sentences or pragraphs), click the small, round link button that appears on the dividing line between Source and Target areas. This will combine the blocks into one section.
- Click ***Click to edit translation***
- In the translation field on the write add or edit translated material
- When complete click the Checkbox
- Note that all sections of the page/post must be translated in order to save
- Once finished, click the ***Complete*** button
-![image](https://user-images.githubusercontent.com/55176130/126729583-99c5b898-9053-4ea7-b36f-5b01a00cdea7.png)
- Note that once any pages have been flagged for translation, they can also be accessed by the Pages or Posts page by clicking on the icon for the page in its Foreign Language Column. You can add, edit, or update pages, depending on current translation status, through these icons
![image](https://user-images.githubusercontent.com/55176130/126730407-fad61350-fd8a-41df-aa93-8716dfdd6662.png)
- Note that other items may appear in the Advanced Translation Editor screen for elements that are not readable on the site (but some which can be seen in the pages source code). Element may include image names, alternate text for images, field names or messagges viewable from the Wordpress Dashboard, or short codes. Image names and alternate text for images are good to translate, however if you do not know what an item is (such as a field name), DO NOT translate it. Doing so could break aspects of the site.

### Editing or Deleting Translations ###
- Translations can be edited following the instructions above.
- If you make changes to any page/post in the original post, you will need to update the foreign language translation
- Note that you must delete translated pages separately. Do this by going to posts or pages, selecting the language at the top and selecting the pages you wish to delete.

### Translate Contact Form 7 ##
- Under WPML -> Translation Management, under the number 1, "Select items for translation" dropdown, select Contact Form and then the **Filter** button
- From here, follow the typical instructions for translation.

### Translate Forms
All forms are made with the Contact Form 7 plugin. To access form content for editing and translation, from the WP dashboard:
- Go to Contact -> Contact Forms, and select the form you wish to edit.
- Under the Form tab, edit any of the titles of your form fields
- In the righthand column, under Translations, click the icon next to the language name
![image](https://user-images.githubusercontent.com/55176130/127945379-4bbb0c11-fd2c-43c0-a8a7-f1eaa2501dcc.png)
- Make and save any translations. See **Translate Post or Page** section of this document for more information.
- If you do not know what the field is, leave the translation the same as it originally was in English.

### Translate Menus ##
Menu items may come either from actual posts or pages accessible in Wordpress, or they may be custom links created by the developer

#### Translate Menus accessible inside Wordpress ###
- Go to Appearance -> Menu
- Click the language link next to "Translations:" on the far right
- Drag items to add to translate menu
- Note that page and post names shown on the Page and Post pages will be in whatever language that is being shown on Appearance -> Menu.

### Translate Custom links ###
Custom links may include external links or links to specific areas on a page or post in Wordpress
- Go to WPML -> Theme and plugins localization
- Under Strings in the themes, check your theme name ("Boostrap Theme")
- Click **Scan selected themes for strings**
![image](https://user-images.githubusercontent.com/55176130/126735173-51bfdede-0e00-45ae-b523-9b59d8ce83bc.png)
- Go to WPML -> String translation
- In the Search for box, type the word or phrase you are looking to translate
- Add the translation
![image](https://user-images.githubusercontent.com/55176130/126735206-124a1ae6-3248-480f-9262-444db8477017.png)

## WPML Troubleshooting ##
- If page does not appear translated, has missing images, or missing content, first verify that a page/post translation exists
- If ACF images appear as broken links on pages:
- 	Go to the page/post and open the editor
- 	Scroll to the bottom and click on Show System Fields
- 	All fields starting with an underscore (ex: `_image-1`) must be set to Copy
- 	If this doesn't work, go to WPML -> Settings -> Media Translation, and click **Start**. Once complete, refresh page and check again.

## General Troubleshooting
1. If content has disappeared from the page, go back to the page or post edit screen and click Update.
2. If you receive a strange message from Google Translate on your page, some custom fields are not showing correctly, or the LANGUAGES link is broken, check to make sure you have the appropriate tag ('english' for English pages) assigned to the page
3. ACF fields not showing in field groups: There might be a corruption with the user. Login as a different user and check again. You may need to delete and recreate the previous user.

## Quick Reference ##
### Edit Page ###
1. Go to Pages --> Edit
2. Edit content, either in upper block section, or in custom fields at bottom of screen
3. Click Update
4. On Page screen, click Tranlate icon
5. Tranlate all fields
6. Confirm changes weremade to both English and Japanese pages

### Add Blog / News Post
1. Go to Posts --> New
2. Add content to bock editor
3. Choose category: Blog or News
4. Upload background image
5. Click Publish twice
5. On Page screen, click Tranlate icon
6. Tranlate all fields
7. Confirm changes weremade to both English and Japanese pages

### Add DJ
1. Create new post per instructions above
2. Add DJ Image
3. Instead of category: Blog or News, click all music genres that apply to DJ
4. Once page content is confirmed, copy URL
5. Go to Users --> Add New
6. Create new User for DJ, selecting Role: Author
7. Enter the DJ page url into the Website field
8. Confirm that DJ appears under DJs section of front page and that it links correctly to the DJ page

### Add Image to Media Library
1. Prior to uploading, crop image
2. Change width to the maximum size necessary for large computer screen
3. Compress image
4. Upload
5. Confirm image title and alt text

# Themes #
## Skitours Theme ##
- Use Post to create a new tour page
- Developer Note: - Template part **content-featured** is only used to embed posts into the home page. Changes to this file will only be reflected on the home page, not within any of the post pages

### Editing Content
- You can change any text or images throughout the site by using either customized fields accessable in each post and page, or directly in the Wordpress page editor, as in the case of the Homepage, Enquiry Information, Company Information, and tour post pages. Note that these are the ONLY pages which should be edited in the native Wordpress editor, Gutenberg. Doing so on other pages could cause issues with the page.
- All images and text in the page must be set for each language page separately.
- Access footer fields at the bottom of the home page.
- Access "OUR PARTNERS" section title and image fields from the bottom of the home page
- Note that Contact page has two types of forms, each in Japanese and English. One is for general enquiries, the other is for specific enquiries about reservations.
- You can edit text and images, but not the structure of the site (except for certain pages listed above, via the native Gutenberg page builder). Adding additional parts to pages (other than the Home page and Company Information page) via the Wordpress editor will likely break the page.
- Using the Preview feature inside page/post edit views will not always show changes correctly. It is better to view changes in a separate browser window, by looking at the actual, published path

Note that any images uploaded should be compressed and optimized for the web. Overly large images will slow your site down.
### Creating New Pages, Posts ###
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


## Adding / Editing Footer Information

If the footer uses ACF then footer information, such as the company address, link titles, or social icons links technically can be edited uniquely for any specific page, but this is ill-advised. When setting up or changing footer content, it is best to bulk edit all pages/posts at once. To do this:

1. Go go Plugins -> Installed Plugins and activate the ACF QuickEdit Fields plugin.
2. Go to Pages and select all pages to edit.
3. In the Footer section select any field would like to change and edit the information.
4. Click Update.
5. Repeat the above procedure for all Posts.
6. Got back to Plugins and Deactivate the ACF QuickEdit Fields plugin (please do not delete it).

### Changing the Homepage Carousel Images
(Note that the Boostrap Theme controls carousel images by ACF, so images can be edited directly in the page edit screen. Only Skitours Theme uses the configuration described here.)

Got to Posts, and you will find posts Slide 1, Slide 2, and Slide 3. The featured image for each post appears as an image in the Homepage carousel. Simply change the featured image to change the carousel.

### Changing the Featured Tour
Use the tag, "featured" to designate any tour you wish to feature on the home page. Be sure to tag both the Japanese and the English post.

### Adding the Featured Products Widget to the Homepage
Under Appearance -> Customize -> Widgets, click Add a Widget, select Featured Product. Then click Publish.

## SSR990 Boostrap-Theme Notes ##
### Template Heirarchy ###
- DJ's and Shows page uses page-blog.php custom page
- All post bages use the single.php template. Inside single.php there is logic to change the look of the blog depending on whether category: DJs has been selected. DJ post pages will display the custom field `background_image` as a banner at the top, as well as the custom field `dj_image`. Otherwise the title will display across the top of the page and `background_image` will display at the top of the body of the post.

# PHP Reference #
Many of the methods described below for used in Wordpress only

## Useful Methods ##
`get_the_category()` returns an array of categories and related data.
