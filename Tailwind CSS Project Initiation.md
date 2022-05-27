This setup uses Tailwind CSS as a PostCSS plugin, in order to also use Autoprefixer to automatically add vendor prefixes.

- `npm init` project
- `npm install -D tailwindcss postcss autoprefixer`
- Create basic project structure:
```
public
  |-index.html
  |-style.css
src
  |-style.css
```
- Add config file: `npx tailwindcss init`
- Make sure the config file content points to the files in which the css will be outputted. These files will be scanned for classes. For example, with the above project structure, the content defined in the config file would look like this: `content: ["./public/**/*.{html,js}"]`
- Add a script to your package.json in order to build your css files. Note that this only has to be done once during a development session:
```
  "scripts": {
    "build": "npx tailwindcss -i ./src/style.css -o ./public/style.css --watch"
  }
```
- Add a PostCSS config file `postcss.config.js`  in the project folder, and add your plugins:
```
module.exports = {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  }
}
```
- Now add classes to your files per Tailwind CSS documentation
