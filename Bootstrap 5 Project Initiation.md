# Project Configuration
- `npm init`
- `npm i sass -D`
- `npm i autoprefixer`
- `npm i --save @fortawesome/fontawesome-free`
- `npm i bootstrap@5.2.0-beta1` (Check for most recent version)
- Create a minimal file structure for sass files to be compiled. Note that it's good practice to use an underscore at the beginning of scss partial files.
```
assets
  |-style.css
scss
  |-style.scss
  |-sections
    |-_navbar.scss
    |-_intro.scss
    |-_other-sections.scss
    |-_footer.scss
  |-components
    |-_animations
    |-_mixins
    |-_typography
index.html
```
- Add the following script to your package.json
```
  "scripts": {
    "build": "sass --watch ./scss/style.scss:./assets/style.css"
  }
```
- Set up your main style.scss (or custom.scss) with the following structure. See **Customization** below for setting up custom variables.
```
// Custom variables

@import "../node_modules/bootstrap/scss/bootstrap.scss

// Import sections and components
```
- Make sure the `head` on all pages includes: `<meta name="viewport" content="width=device-width, initial-scale=1">`

# Customization
Variables can be found in `node_modules/bootstrap/scss/variables`. Note also that svg icons are best downloaded or copied from the Bootsrap Icons page. (See 34:00 from https://www.youtube.com/watch?v=iJKCj8uAHz8) for details on copying html paths for svg icons.
- Copy any variables you want to use into the top of the `style.scss` file, above your bootstrap.scss import
- Remove and `!default` value from the variable, so they will override those found in the main bootsrap.scss file



