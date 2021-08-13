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
- **Be sure to check your actual post online and make sure all images are showing on the translated page** If they are not, see the WPML Troubleshooting secitn below.

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
- If images appear as broken links on pages: Go to WPML -> Settings -> Media Translation, and click **Start**. Once complete, refresh page and check again.

## General Troubleshooting
1. If content has disappeared from the page, go back to the page or post edit screen and click Update.
2. If you receive a strange message from Google Translate on your page, some custom fields are not showing correctly, or the LANGUAGES link is broken, check to make sure you have the appropriate tag ('english' for English pages) assigned to the page

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
2. Instead of category: Blog or News, click all music genres that apply to DJ
3. Add DJ Image
4. Add Background Image
5. At bottom of screen 
6. Once page content is confirmed, copy URL
7. Go to Users --> Add New
8. Create new User for DJ, selecting Role: Author
9. Enter the DJ page url into the Website field
10. Under post -> Quick Edit, change author of post to DJ name
11. Translate post
12. Confirm that DJ appears under DJs section of front page and that it links correctly to the DJ page
13. Confirm DJ pages show correct information in both languages

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
