# Terminology
1. Basic Syntax
  - A **hash** is an associative array of unique keys and their values
    - Alternative syntax can be used when defining the key names of a hash, using `:` at the end of the hash name, thus converting the key name into a symbol.
    ```
    { foo: 1, bar: 2 }
    #=> {:foo=>1, :bar=>2}
    ```
  - A **symbol** is a literal, static name that doesn't change, and is designated by a `:` in front of the name. Example: `:articles`. Controller names, for example, are defined as symbles. Symbols are memory efficient
3. **Routes** - Connect incoming HTTP requests to application and generates URLS (no hard coded URLS)
  - **routes.rb** is a single block sent to `ActionController::Routing::Routes.draw`
  - Routes assign a controller and an action to a URL
  - URLs are mapped by the controller's action (GET, POST, DELETE, PATCH, PUT)
  - `resources` routings iwll difine all common routes for a controller
  - RESTful routes use the following declaration style: `resources :controller-name`

# Initial Setup
Assuming you have Ruby and Rails already installed:
1. Create your app: `rails new blog --skip-spring --skip-listen` (Note: this is specifically for Linux running on a Windows subsystem)
3. Start app: `bin/rails server`


