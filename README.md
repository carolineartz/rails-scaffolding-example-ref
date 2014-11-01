Scaffolding rails project example/reference files.
-
Again, I recommend learning to write what goes in these files at the beginning, but having an example is great as a reference.
## Creating project files, basic order of doing things


###  Create Project
```ruby
#this is what we did 
rails new blog -T -d sqlite3

#this is what i actually do most of the time
rails new -T --database=postgresql
```

flags:
`-T`: don't generate `Test::Unit` files--use rspec by adding the rspec-rails gem.
`-d` or `--database=`: database type. I usually use postgresql...but whatever you use, you can specify it here. sqlite3 is default, so you don't actually need to do anything for that.

Add gems to your Gemfile **_before_** you start generating models...many good ones of these gems take action with the Rails generators

- rspec-rails will automatically generate specs, 
- factory_girl_rails will automatically generate factories

### Create the database

```ruby
rake db:drop #just to avoid erroring out from an old database instance
rake db:create 
```
### Generate a model & migration

```
rails generate model Article
```
* You can add fields to this command, or add them to the generated file. 

#### Generated:

`db/migrate/20141101012944_create_articles.rb` #database migration file
`app/models/article.rb` #The file that will hold the model code

      create      spec/models/article_spec.rb

`db/migrate/(some_time_stamp)_create_articles.rb` 
`app/models/article.rb` #
`test/models/article_test.rb` #A file to hold unit tests for Article
`test/fixtures/articles.yml` #A fixtures file to assist with unit test

note on the second 2 files above: this will create different files using rspec, which i added later on in our walk-through, but which you should add before generating anything.

- since we didn't add fields via the command line, we need to flesh out the model by adding this to the migration file.
 
```ruby
def change
  create_table :articles do |t|
    t.string :title
    t.text :body

    t.timestamps
  end
end
```

run
```
rake db:migrate
```

Check out the rails console

```
rails c
```
play around with adding to the database

### Create your resources => Generates routes
```ruby
Blog::Application.routes.draw do
  resources :articles
end
```
run...

```
rake routes
```
and see what routes were generated when you include all resources (by not adding include/exclude specifications).

Prefix Verb   URI Pattern                  Controller#Action
articles      GET    /articles(.:format)          articles#index
              POST   /articles(.:format)          articles#create
new_article   GET    /articles/new(.:format)      articles#new
edit_article  GET    /articles/:id/edit(.:format) articles#edit
article       GET    /articles/:id(.:format)      articles#show
              PATCH  /articles/:id(.:format)      articles#update
              PUT    /articles/:id(.:format)      articles#update
              DELETE /articles/:id(.:format)      articles#destroy

## Generate controller
```ruby
rails generate scaffold_controller articles
#or 
rails generate controller articles
```

CREATES:
`app/controllers/articles_controller.rb` # The controller file itself
`app/views/articles` # The directory to contain the controller’s view templates
*`test/controllers/articles_controller_test.rb` # The controller’s unit tests file
`app/helpers/articles_helper.rb` # A helper file to assist with the views (discussed later)
*`test/helpers/articles_helper_test.rb` # The helper’s unit test file
`app/assets/javascripts/articles.js.coffee` # A CoffeeScript file for this controller
`app/assets/stylesheets/articles.css.scss` # An SCSS stylesheet for this controller

won't be there using Rspec

