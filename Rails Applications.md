# Terminology
## CS Terminology
- A [**DSL**](https://www.toptal.com/ruby/ruby-dsl-metaprogramming-guide), or domain specific language, is a programming language specific to certain application domains and is not general use. Some frameworks build higher language-syntax that accesses another langauge, becoming an _internal DSL_. Ruby is an example of a DSL, and Rails uses its own internal DSL based on Ruby.

## Ruby/Rails Terminology
  - A **hash** is an associative array of unique keys and their values
    - Alternative syntax can be used when defining the key names of a hash, using `:` at the end of the hash name, thus converting the key name into a symbol.
    ```
    { foo: 1, bar: 2 }
    #=> {:foo=>1, :bar=>2}
    ```
  - A **symbol** is a literal, static name that doesn't change, and is designated by a `:` in front of the name. Example: `:articles`. Controller names, for example, are defined as symbles. Symbols are memory efficient
  - An **instance variable** (ex.: `@variableName`) is restricted to the object that itself refers to
  - A **method** is just a function. Many methods are built-in, but can also be created. You can pass parameters to a method with or without parentheses:
  ```
  add_two 2
  add_two(2)
  ```
  - **Parameters** are arguments passed to a method. In a web app there are two types: 1) query string parameters, which received from the browser URL in a GET request, and 2) POST data which comes from form info entered by the user. Rails treats the two types the same, using the `params` hash
  - **params** is a built in hash that allows access to an object's properties. For example, a controller's `show` action may be defined using params:
  ```
    def show
    @article = Article.find(params[:id])
  end
  ```
  Here, when a GET request is made (show action), an instance varialbe `@article` is created. The article is defined by running a `find` method on the Article model (a table full of articles). Find is passed a params hash with the value of the ID defined from the URL

## Model
[**Active Record**](https://medium.com/@moulayjam/what-is-rails-activerecord-2a891390703b#:~:text=Rails%20Active%20Records%20provide%20an,field%20names%20of%20database%20tables) is the Model (M) of the Rails MVC configuration. It manages data and business logic and connects your application to the database, allowing you to make programmatic changes to the database.

- A model is a Ruby class that can add, find, update, and remove data from a database (CRUD). Rails can automatically generate models.
- Use the generate command to build a model:
  ```
  bin/rails generate model ModelName ColumnOneName:ColumnOneType ColumnTwoName:ColumnTwoType
  ```
  Ex.: `bin/rails generate model Article title:string body:text`
- The above model generation **creates the constructor class CreateArticles for the article object** This object will later be accessed by used of an instance varialbe in the controller
- Generating the model will also generate a migration file. Migrations define changes that will be made to the database. Migration files defined in Ruby are database-agnositc
- If you are adding **associations** make sure to do so when creating the model. (See section below.)

### Validations
Validations control what kind of data can be added to the database. Add validations to the model file under `app/models/modelName.rb`
```
class User < ApplicationRecord
  validates :name, presence: true, uniqueness: true, length: { in: 4..16 }
end
```

## Migration File
Upon execution, migration files give commands to the database on how to change that database. Once a migration file is created, `rails db:migrate` must be performed. Once a migration is complete **do not** make changes to any previous migration files. A new migration file will need to be created and run (see below).

Here is an example of a migration file automatically generated when a model was generated:
```
class CreateArticles < ActiveRecord::Migration[6.1]
  def change
    create_table :articles do |t|
      t.string :title
      t.text :body

      t.timestamps
    end
  end
end
```
- `CreateArticles` was automatically generated when the `Article` model was generated. `CreateArticles` inherits the functionality of ActiveRecord's Migration class. The Migrations class in ActiveRecord contains helper methods that perform CRUD operations, like addtable, addcolumn, and rename_table. These are the types of things we want our class to do, so it inherits those methods, giving our class the ability to perform them.
- The `change` method is defined by default as a constructive migration method. It will generate a table with the specified columns as well as timestamps for creation and updates
- You must run `db:migrate` in order to update the database with the new migration

### Additional Notes
- Adding an index or indexes to a table can help with querying. `add_index` can be used. The following command adds an index to the `:userName` column of `:users`:
```
def change
    add_index :users, :userName
  end
```

### [Adding columns to a table](https://stackoverflow.com/questions/4834809/adding-a-column-to-an-existing-table-in-a-rails-migration)

### Troubleshooting the database
-**ActiveRecord:InvalidForeignKey** If you try to destroy a record and get this error, it is because you are being constrained by a foreign key association. If you wish to delete all associated files, and `on_delete: cascade` to the foreign key and run a new migration. Example:
```
class AddForeignKeyToComments < ActiveRecord::Migration[6.1]
  def change
    add_foreign_key :comments, :articles, on_delete: :cascade
  end
end
```

## Associations to a model
Associations connect multiple tables in a database, making them relational. 
- Include your foreign key when setting up your model and migration, you can add `user_id` as a foreign key to the Post model at the point of generation:
`bin/rails generate model Post title:string body:text user_id:integer`
- Add associations under the appropriate model files:
```
class User < ApplicationRecord
  has_many :posts
end

class Post < ApplicationRecord
  belongs_to :user
end
```

## Controller
The controller uses the model to access the database in some way
- Controllers can be generated in the bash terminal using the following:
  `bin/rails generate controller ControllerName actionName --skip-routes`. Note that `--skip-routes` is optional
- Once the controller is generated, the controller file will be generated. Ex: `app/controllers/articles_controller.rb`
- Additionally, a view will be generated by default. Ex: `app/views/articles/index.html.erb`
- Here is a typical controller script:
```
class ArticlesController < ApplicationController
  def index
    @articles = Article.all
  end

  def show
    @article = Article.find(params[:id])
  end

  def new
    @article = Article.new
  end

  def create
    @article = Article.new(article_params)

    if @article.save
      redirect_to @article
    else
      render :new
    end
  end

  def edit
    @article = Article.find(params[:id])
  end

  def update
    @article = Article.find(params[:id])

    if @article.update(article_params)
      redirect_to @article
    else
      render :edit
    end
  end

  def destroy
    @article = Article.find(params[:id])
    @article.destroy

    redirect_to root_path
  end

  private
  def article_params
    params.require(:article).permit(:author, :title, :body)
  end
end
```

### Accessing data via instance variables in the Controller
The Controller makes use of **instance variables** (ex.: `@articles`). When an action is triggered (E.g.: user clicks on New button), an instance of the controller is created and the `new` method is called. At that point, any instance variables defined are available to any method or view that is the result of that action. **This is especially important for connecting variable data between the controller and the view**

Instance variables will typically be defined as a model (table) with a method called on it. Ex.: `@articles = Article.all`

### Using strong parameters
when using the create action, you are allowing the user access to every parameter in your table, which is dangerous. Instead you must use a private method, and define only those parameters you are allowing access to:
```
  private
  def article_params
    params.require(:article).permit(:title, :body)
  end
end
```

## Routes
Connect incoming HTTP requests to application and generates URLS (no hard coded URLS)
- **routes.rb** is a single block sent to `ActionController::Routing::Routes.draw`
- Routes assign a controller and an action to a URL. I.e.: routes will map a URL _to_ an action _of_ a controller
- URLs are mapped by the controller's action (GET, POST, DELETE, PATCH, PUT)
- `resources` routings iwll difine all common routes for a controller
- RESTful routes use the following declaration style: `resources :controller-name`
- Models that are dependent upon other models must be nested
- A **resource** includes all of the following built in RESTful routes:
```
bin/rails routes
      Prefix Verb   URI Pattern                  Controller#Action
        root GET    /                            articles#index
    articles GET    /articles(.:format)          articles#index
 new_article GET    /articles/new(.:format)      articles#new
     article GET    /articles/:id(.:format)      articles#show
             POST   /articles(.:format)          articles#create
edit_article GET    /articles/:id/edit(.:format) articles#edit
             PATCH  /articles/:id(.:format)      articles#update
             DELETE /articles/:id(.:format)      articles#destroy
```
- Use `resources` in you routes like so (you can use the `only` option to use only specific routes):
```
Rails.application.routes.draw do
  resources :users, only: [:new, :create]
end
```

# Setup and deployment
Assuming you have Ruby and Rails already installed:
1. Create your app: `rails new blog --skip-spring --skip-listen` (Note: this is specifically for Linux running on a Windows subsystem)
3. Start app: `bin/rails server` to confirm it is connected
4. Add your first route, for example:
  ```
  Rails.application.routes.draw do
  root "articles#index"
  end
  ```
5. Generate your controller from bash terminal: `bin/rails generate controller Articles index --skip-routes`
6. Generate your model: `bin/rails generate model Article title:string body:text
7. Run your migration: `bin/rails db:migrate`

## VS Code Environment
Make sure you have ERB Formatter/Beutify and Ruby Extensions installed. Add the following for htmlbeautifier and emmet in you Workspace.
In your workspace settings, add the following:
```
{
  "ruby.useBundler": true, //run non-lint commands with bundle exec
  "ruby.useLanguageServer": true, // use the internal language server (see below)
  "ruby.lint": {
    "rubocop": {
      "useBundler": true // enable rubocop via bundler
    },
    "reek": {
      "useBundler": true // enable reek via bundler
    }
  },
  "ruby.format": "rubocop", // use rubocop for formatting
  "files.associations": {
  "*.erb": "erb"
  },
  "[erb]": {
    "editor.defaultFormatter":"aliariff.vscode-erb-beautify",
    "editor.formatOnSave": true
  },
  "[html]": {
    "editor.defaultFormatter": "aliariff.vscode-erb-beautify",
    "editor.formatOnSave": true
  },
  
  "emmet.includeLanguages": {
          "erb": "html"
        },
  "emmet.showAbbreviationSuggestions": true,
  "emmet.showSuggestionsAsSnippets": true,
}
```

## Deployment to Heroku
Before beginning, you should have already configured your command line for heroku commands, created a project, and initiated git. Then navigate to your project folder. Inside the project:
- Enter `heroku create`
- Enter `git remote` and check that heroku files are there
- In your Gemfile, replace the line `gem 'sqlite3' with:
```
group :development, :test do
 gem 'sqlite3'
end

group :production do
  gem 'pg'
end
```
- From command line enter: `bundle config set --local without production`
- Enter `bundle install`
- Commit changes to git and Github
- Push to Heroku: `git push heroku main`
- Migrate database to Heroku: `heroku run rails db:migrate`
- You should be all set up. Use `heroku open` to open the site and verify!

# The Form Builder
- Forms will be found under `app/views/articles/new.html.erb `
- The form will be wrapped in the following:
```
<%= form_with model: @article do |form| %>
...
<% end %>
```
- `form_with` is a helper method that instantiates the form
- `:model` is passed to `form_with` as a resource, with all its RESTful routes
- @article instance variable will pass the @article URL and POST route on submit
- If using a form as a partial replace the instance variable (`@article`) with a local variable (`article`).  Because partials are shared code, it is best practice that they do not depend on specific instance variables set by a controller action. 

# Partials
- Partials should be saved under the appropriate veiws folder, and the title should begin with an underscore. Ex.: `app/views/articles/_form.html.erb`
- Use `render` to access the partial: `<%= render "form", article: @article %>`

# Helpful methods and modifying the database via irb
You can access your database directly through irb on the console. `bin/rails console` will open irb. Use `reload!` to reload the console after making any changes to model files

- `all` method returns an array of all records in a table. Ex.: `User.all`
- `new` method creates an record instance. Ex.: `u = User.new` will create a new user, but not save it to the database
- Add attributes to new User: `u = User.new(name: "John")`
- `create` method creates and saves a new record to the database. Ex.: `u = User.create` will actually add a new record
- `valid?` checks for validations. Note that a record with no validations (Ex.: `u.valid?` for the above example returns `true` because it has not validation criteria assigned to it).
- `destroy` destroys the record. Ex. : `u = User.last` sets variable to last record in Users. `u.destroy` will destroy that last record.
- 'recordName.errors.full_messages` show full description of any errors.
- `save` method saves a new instance to the database. Ex.: `u.save`
- `update` updates the value of an existing record. Ex.: `Article.last.update(user_id: 1)` will update the last Article record `user_id`

# Using Devise for authentification [Link](https://github.com/heartcombo/devise)
- Add the following to your Gemfile: `gem 'devise'`
- Run `bundle` from command line
- Run `rails generate devise:install` from command line
- Add `config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }` to `config/environments/development.rb`. This will define default url options for development environment
- Make sure you have a root set up already before running devise
- Add `rails g devise:views` for a variety of default views. This should especially be done if devise is being used with bulma
- Type `rails generate devise User` to generate a User model, migration and route
- Run `db:migrate`
- Go to `path/users/sign_up` to verify Sign Up page has been generated
- Go to Articles controller and add to the top of the file: `before_action :authenticate_user!, except: [:index, :show]`
- Add `registrations_controller.rb`
```
class RegistrationsController < Devise::RegistrationsController

  private
  def sign_up_params
    params.require(:user).permit(:name, :username, :email, :password, :password_confirmation)
  end
  
  def account_update_params
    params.require(:user).permit(:name, :username, :email, :password, :current_password)
  end
  
end
```
- Check `routes.rb`. You should see `devise_for :users` at the top. Change this to `devise_for :users, :controllers => { registrations: 'registrations' }`. If this isn't done, then a new user role's name and username will be set to `nil`.
- Add `rails g migration AddFieldsToUsers`
- Add the following into your AddFieldsToUsers migration:
```
def change
  add_column: :users, :name, :string
  add_column: :users, :username, :string
  add_index :users, :username, unique: true
end
```
- Run `db:migrate`
- Make necessary edits to `views/devise/registrations/new` There are many other views you may need to update as well.
- Go to Article model and add `belongs_to :user`.
- Go to User  model and add `has_many :articles`
- Add user id to articles model: `rails g migration AddUserIdToArticles user_id:integer`
- Run `rails db:migrate`
- Use `current_user` helper in the Articles controller. Under both `new` and `create` actions, replace `Article.new` with `current_user.articles.build`. Note that you can also use this helper in embedded ruby inside any view: `<%= current_user.name % >`
- Route new user sign up to got ot Registrations controller.
- At this point, confirm that you can sign up for a new account by signing up on the site. Got to `path/users/sign_up`. You can confirm by checking `@user.last` in the rails console
- Remove any test articles not tied to any account
- Connect appropriate article content to name, username, etc. For example, to show the username of the author on the article, use `<% article.user.username %>`
- Prevent user from being prompted to sign in again, with the following, added either to the navbar section:
```
<% if user_signed_in? %>
  <%= link_to current_user.name, edit_user_registration_path, class: "classes" %>
  <%= link_to "Logout", destroy_user_session_path, method: :delete, class: "classes" %>
<% else %>
  <%= link_to "Sign In", new_user_session_path, class: "classes" %>
  <%= link_to "Sign Up", new_user_registration_path, class: "classes" %>
<% end %>
```
- Note: You can use `user_signed_in?` method with an if statement to reveal various parts of a view, depending on whether they are signed in.

## Using console to confirm table data
- `@user = User`
- `User.connection`
- Type `@user` again to see all columns
- Type `@article = Article` to see all columns under Article. You should see a `user_id` key.

## Notifications
Devise uses flash notices to tell the user whether the their login was successful or unsuccessful. Place the following into the top of any pages where you have a sign in, just inside the `body` tag:
```
<% if flash[:notice} %>
  <p class="notice"><%= notice %></p>
<% end %>
<% if flash[:alert} %>
  <p class="alert"><%= alert %></p>
<% end %>
```

#Custom 404 Page
Rails ships with 404 pages, but all css must be embedded in the page. A better option is to set up custom pages.
- Add a controller with methods: `rails g controller errors not_found internal_server_error`
- In the controller:
```
def not_found
  render status: 404
end

def internal_server_error
  render status: 500
end
```
- In `routes.rb` add:
```
match "/404", to: "errors#not_found", via: :all
match "/500", to: "errors#internal_server_error, via: :all
```
- Under `config/application.rb`, under `module DynamicErrors`, add:
```
config.exceptions_app = self.routes
```
- Delete the public files
- To be able to see the changes in development environment, go to `config/environments/development.rb` and change `config.consider_all_requests = true` to `false`. **Make sure to change back before going to production**










