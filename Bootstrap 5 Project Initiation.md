- `npm init`
- `npm i sass -D`
- `npm i autoprefixer`
- `npm i --save @fortawesome/fontawesome-free`
- `npm i bootstrap@5.2.0-beta1` (Check for most recent version)
- Create a minimal file structure for sass files to be compiled:
```
assets
  |-style.css
scss
  |-style.scss
index.html
---
- Add the following script to your package.json
```
  "scripts": {
    "build": "sass --watch ./scss/style.scss:./assets/style.css"
  }
```
- Import bootstrap files into `style.scss` by adding at the top of the file: `@import "../node_modules/bootstrap/scss/bootstrap.scss"`
