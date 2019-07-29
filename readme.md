# Welcome to Rails!

## What is Rails?
So we've learned about Sinatra, your very first web framework! It's an awesome tool to get a simple web app up and running quickly. So why would we ever want to learn Rails?
If Sinatra is the equivalent of a screwdriver then Rails is a complete garage of powertools. You have to learn to handle the screwdriver before you get to use the powertools.
###### Sinatra
![SinatraKit](https://www.geekvape.com/store/media/wysiwyg/Geekvape_Simple_Tool_Kit.jpg)
###### Rails
![Railskit](http://ethermeals.co/wp-content/uploads/2018/12/garage-workshop-design-home-decor-workshops-fresh-garage-workshop-design-ideas-at-home-garage-workshop-plans-uk.jpg)

Many of the conventions previously used in Sinatra will transition easily into Rails, except there are exponentially more of them.  Rails is a heavy duty web framework that emphasizes **convention over configuration** in order to streamline the  development process. 

> Rails is a web-application framework that includes everything needed to create database-backed web applications according to the Model-View-Controller pattern.

> It is designed to make programming web applications easier by making assumptions about what every developer needs to get started. It makes the assumption that there is the "best" way to do things, and it's designed to encourage that way - and in some cases to discourage alternatives. - Ruby on Rails guide


## MVC

![MVC](http://i.stack.imgur.com/Sf2OQ.png)

The design pattern that Rails is built around is rMVC - router, model, view and controller.

### Life Cycle of the request/response in Rails:
1. A user of our web application submits a request to our application's server. It can come in a myriad of ways. Maybe someone typing in a URL and hitting enter or maybe a user submitting a form on our application.

2. The request hits the router of the application.

3. The application then either doesn't recognize the route (error) or it does recognize it(route) and sends it to a controller.

4. Once the request hits the controller, it's then going to designate a model to deal with the database. Our models will be using ActiveRecord in our case.

5. The model, using ActiveRecord, will be used by the controller to generate  an ActiveRecord object to query the database for the information specified in the controller. The ActiveRecord object then stores the results of the query.

6. The ActiveRecord object then returns all the data it so helpfully grabbed from the database to the controller.

7. Once the controller has the information from the model that it needs, it sends it to the view. The view is fed the objects (i.e. data & instance variables) by the controller and grabs the appropriate rendered view template.

8. The rendered view is sent by the controller as a **response** to the user **request**.
---
# Learning Rails

Rails is known for being 'magical', but under the hood it is really nothing you don't already know.  In fact, the core principles of building a website in Rails are very similar to Sinatra - except Rails abstracts away much of the menial work and structures everything according to conventions. 

Let us examine some Rails code and break things down:

### Creating a Rails App
Let's jump into it.  We'll install Rails and create a Rails skeleton app called `first-rails`:

```bash
gem install rails -v 6.0.0.rc2
rails new first-rails -d postgresql
```


By default, a new Rails project is preconfigured with a sqlite3 database. Lets add `-d postgresql`  to setup a postgresql database instead.

 If you want to read more about configuring your database, read [this](http://edgeguides.rubyonrails.org/configuring.html#configuring-a-database).  It explores the difference between the development, test and production environments of a typical Rails app.

You should see many folders and files generated from just that one command in the `first-rails` folder.  The layout should be relatively familiar to you already - with the addition of several new files and folders.

## Rails folder/file structure

The huge folder structure will take some getting used to, but most of it should seem similar to Sinatra. Rails prizes "convention over configuration" -  conventions in such a massive framework allows us to build and scale quickly.
Rails is exceptionally particular about how we name our folders and files. Let's review some of these conventions:

The first folder we'll look at is  `app` :

This folder is the the most important folder in your entire application containing most of the programs functionality.
- `assets` - This will be where all of your CSS, JS, and image files belong. Prior to Rails 6, JavaScript files were also stored here.
- `channels` -  Contains your ActionCable code.
- `controllers` - Contains the controllers.
- `helpers` - This is where you'd define helper methods for your app.
- `javascript` - Contains all JS files and JS packages  (Rails 6+ only).
- `jobs` - Contains ActiveJob code.
- `mailers` - Contains Mailer code: to send and receive email within a Rails app.
- `models` - Contain the ActiveRecord models.
- `views` - Contains all of the views (in html.erb).

> These folders are easily the most important part of your application.

The `bin` folder contains binstubs. Binstubs are used as wrappers around ruby gem executables to be used in lieu of `bundle exec`.

The `config` is contains the app's various configuration, intializer, environment and YAML files, including  `routes.rb`, the router in MVC.

The `db` folder contains all migration and seed files. 

In the main directory you will find :

-  `Gemfile` and `Gemfile.lock` - These are used by bundler to know which versions of RubyGems are used in your application.
-  `babel.config.js` and `postcss.config.js` - These are configuration files for babel and postcss.
-  `package.json` and `yarn.lock` - These adeal with the npm (javascript) packages that you have installed.

The `rake` tasks that you have been using now use the `rails` prefix:

```
rails db:create
rails db:migrate
rails console
```

To start the server, instead of typing  `shotgun`, you now use:
```
rails server
```
or simply:
```bash
rails s
```

Some important things to note:
  * Rails will automatically reload changes.
  * Rails has far more detailed logs than sinatra, so make good use of them in debugging your problems.
  * Rails runs on localhost:3000 instead of locahost:9292.

After running `rails server`, check `localhost:3000` on your browser to see the generic Rails welcome page.

### Models

Models in Rails work exactly the same as they would in Sinatra. ActiveRecord is native to Rails. Make sure your model files are in the `app/models` folder in the Rails app. 

Always remember the convention - the name of the file has to be singular in snakecase. The name of the class has to match the name of the file, in CamelCase.

Example:

`app/models/article.rb`:
```ruby
class Article < ActiveRecord::Base

end
```

You may use the generator to generate models:
```bash
rails generate model Article title:string text:string
```

Surprise! The generate model command also creates corresponding migration files for your model.

### Database and Migration Files

When you generate a model, the accompanying migration file will automatically be generated for you. However, you may choose to generate migration files with `rails generate migration` such as:

```bash
rails generate migration AddAuthorToArticle author:string
```

As always, remember to:

```bash
rails db:create
rails db:migrate
```

Let's run `$ rails console` and play around with our models to test that all connections are working properly.

---

## Routes (the non Rails way - Explicit Declarations)
> Note that we are defining routes very explicitly in this section. This isn't the Rails way to do this.

Now that our database connection is established, let's move on to building our routes.
Similar to our controllers in Sinatra, we are mapping out the different routes of our app, beginning with the Artist's controller. 


In Sinatra, you'd have in `application_controller.rb`:
```ruby
get '/articles' do
 # [...]
end
```

In Rails, the routing is separated from the controller code. You can still declare the same kind of match conditions like `get '/'` or `post '/create'`, but you would have to specify a separate `controller` class and `action` for your code.  Let's add a similar route as the above in `config/routes.rb`.

In Rails, this would look like:
```ruby
# in config/routes.rb
FirstRails::Application.routes.draw do
  get '/articles' => 'articles#index'
end
```

The  `articles#index` is referencing the `ArticlesController` (not yet created) and the `index` action.  

### RESTful Routes (The Rails way - Using Resources)

 Rails strongly encourages RESTful resources, which means exposing 7 conventional routes that map to 7 actions in the controller. Read sections 2.1, 2.2 and 2.3 from [here](http://edgeguides.rubyonrails.org/routing.html). Occasionally, not all of the pages on your app fit into the conventions of a resource, so you can create other (non-RESTful) methods in the controller for handling those situations.

## Controllers

You may have noticed there was  `app/controllers/application_controller.rb`  already.  This is a class that all controllers inherit from and houses common controller code.  An example of which might be authentication code, which most controllers would want in a typical app.

An `action` is really just a method on your controller.

```ruby
# in Sinatra 
get '/articles' do
  # Look in app/views/articles_index.erb
  erb :articles_index
end
```

As mentioned before, Rails separates the routing into `routes.rb`.  The code that previously was in the `do` `end` block can now be placed in the `index` method. Create a `app/controllers/articles_controller.rb` as follows:

```ruby
# in app/controllers/articles_controller.rb
class ArticlesController < ApplicationController
  def index
    @example = params[:example]
    # Look in app/views/articles/index.erb
  end
end
```

- `params` works the same as it did in Sinatra.
- `redirect()` statements in Sinatra are `redirect_to()` in Rails.
- Instead of `erb(:some_resource)` statements,  use the `render()` statement *or omit them if the name of the file matches the controller and action name*.

You can learn more about controller objects [here](http://guides.rubyonrails.org/action_controller_overview.html).  

## Views

There are three differences from Sinatra to note:
  1. The view files are now within another subfolder.  The folder name should correspond with its controller's name, in this case "pages".
  2. The file name is now `index.html.erb` instead of `index.erb`.  Sinatra assumed HTML-only resources whereas Rails can handle multiple responses of different types out-of-the-box, like Javascript or XML. Hence, why HTML is specified in the file name.
  3.  `erb :index` is redundant; Rails assumes it will find a file with the same name as the action in a folder with the same name as the controller.

Create `index.html.erb`, add `<h1>Hello World</h1>`, and save it.

Learn about about views [here](http://edgeguides.rubyonrails.org/layouts_and_rendering.html).

Now, refresh `localhost:3000` . Congratulations! You have just built a basic Rails app. 

## Conventions

Note the conventions here. We created an `articles` folder to put the `index.html.erb` inside. When we define an `action` (a method) in our controller, Rails will render the view corresponding to the controller and action. 
In this example, because we are calling the `index` action in the `articles_controller`, Rails will look for the `index` view in the `articles` folder.

None of this is new to you. Rails simply automates the drudge work to maximise your developer happiness!

### Generators

Rails provides commands for conveniently creating all components of a RESTful resource.  We **strongly discourage** you from using them now because you will miss what is happening and you will not be able to learn the different components in Rails. For now, we will only use the following `rails generate` commands:
```bash
rails generate model
rails generate controller
rails generate migration
```
There are more, but we will explore this mid-way through Rails.

## Route Helpers

[Section 2.3](https://edgeguides.rubyonrails.org/routing.html#path-and-url-helpers) of the [routing guide](https://edgeguides.rubyonrails.org/routing.html) mentions routing helpers which are helper methods available in your views and controllers that will help automatically create the string for a URL or path.  
This is preferable to typing the strings directly as any changes in links can be done at the route level instead of changing every link string in your code.  Make sure to be comfortable and familiar with these helpers.

## View Helpers

There are a ton of helper methods that you can use in your view:

* [link_to](http://api.rubyonrails.org/classes/ActionView/Helpers/UrlHelper.html#method-i-link_to)
* [image_tag](http://api.rubyonrails.org/classes/ActionView/Helpers/AssetTagHelper.html#method-i-image_tag)

Take a moment to read the documentation.

The idea is to keep developers writing more Ruby and less HTML, and these helper methods will generate the HTML required to solve basic functions. The `link_to` helper for example will 'figure out' its corresponding URL based on RESTful conventions. 

There are many other View Helpers to explore.  While it isn't important to know all of them, knowing them will make your developer life much easier. 

## Form Helpers

Form helpers tend to be confusing at the start.  Like all the other helpers, they save you the trouble of writing HTML: in this case, forms and inputs.

Another  point of confusion is that there are two ways of writing form helpers.  `form_tag` and the related *_tag helpers just save from you writing HTML.  However, the `form_with` (`form_for` in Rails < 6) helper takes an object as an argument and tries to construct a form that intelligently extracts all the relevant data from the object.

Read the entire section on [form helpers](https://edgeguides.rubyonrails.org/form_helpers.html).
