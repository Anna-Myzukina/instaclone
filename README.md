# Build an Instagram Clone with Ruby and Pusher
Read more in this [article](https://dev.to/troy34/build-an-instagram-clone-with-ruby-and-pusher-54ik)

### Setting up the application
` I built the app using Ruby on Rails. I made use of Devise for user authentication and Carrierwave for image upload.
If you have Ruby and Rails installed and want to follow along, run the following command to generate a new Rails app.`

* git undo all uncommitted or unsaved changes 

        git reset --hard HEAD

* NOTE before start chreate project using this command (name_of_project it`s example, you should change it on your own name of your project for example: facebook-clone...)

        rails new name_of_project --database=postgresql
       
* NOTE itis very important!!! You should install postgresql in your operating system! Next article should help: [How To Install and Use PostgreSQL on Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-18-04)
        
* This article and video can be useful [The Ultimate Intermediate Ruby on Rails Tutorial: Let’s Create an Entire App!](https://www.freecodecamp.org/news/lets-create-an-intermediate-level-ruby-on-rails-application-d7c6e997c63f/) , also this article [Adding Authentication with Devise](https://guides.railsgirls.com/devise) and video: [Testing with RSpec](https://www.youtube.com/watch?v=71eKcNxwxVY)

* NOTE If you forgot during creating your app add next peace of code --database=postgresql please follow next article [Making the Change From SQLite3 to PostgreSQL - Ruby on Rails](https://dev.to/torianne02/making-the-change-from-sqlite3-to-postgresql-ruby-on-rails-2m0p) and add postgresql manually : But main you should add lat version of postgress.

* NOTE if you need to add column to your database please use next command

        rails generate migration add_fieldname_to_tablename fieldname:string. 
        
        
* NOTE Sometimes, even dropping a local development database is not a good idea. There are better ways to delete/destroy a specific migration in your Rails application.

You could use rails d migration command to destroy a particular migration:

rails d migration MigrationName
To undo the changes corresponding to a particular migration, you can use db:migrate:down method like this:

        rake db:migrate:down VERSION=XXX

* version you can find in file schema.rb

after you that command you delete that file donot forgor to run

        rails db:migrate
        
        
### Testing https://leanpub.com/everydayrailsrspec/read_sample

* In this project I used postgres kindly recommended at first to read next article [How To Set Up Ruby on Rails with Postgres](https://www.digitalocean.com/community/tutorials/how-to-set-up-ruby-on-rails-with-postgres) cause you need to install postgress in your invironment(laptop or PC)
In your app's root directory, open your Gemfile and add the following and then run bundle install in your terminal:


* [to install Yarn](https://classic.yarnpkg.com/en/docs/install/#debian-stable)


* On Debian or Ubuntu Linux, you can install Yarn via our Debian package repository. You will first need to configure the repository:


    curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -

    echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list

    sudo apt update && sudo apt install yarn




* rails new instaclone -T --database=postgresql

        gem 'bootstrap', '~> 4.1.0'
        gem 'jquery-rails'
        gem 'pusher'
        gem 'figaro'
        gem 'devise'
        gem 'will_paginate', '~> 3.1.0'
        gem 'carrierwave'
        gem "fog-aws"

* bundle install

### Database setup
To get our app up and running, we’ll create a database for it to work with. You can check out this article on how to create a Postgres database and an associated user and password.

Once you have your database details, in your database.yml file, under the development key, add the following code:

        # config/database.yml

        ...
        development:
        <<: *default
        database: instaclone_development // add this line if it isn't already there
        username: database_user // add this line
        password: user_password // add this line
        ...


Ensure that the username and password entered in the code above has access to the instaclone_development database. After that, run the following code to setup the database:

#    setup database
$    rails db:setup

### User authentication
With our database set up, we'll set up user authentication with Devise. Devise is a flexible authentication solution for Ruby on Rails. It helps you set up user authentication in seconds. In your terminal, run the following command:

* rails generate devise:install

#    generate Devise view pages
$    rails generate devise:views

#    generate user model
$    rails generate devise user

#    generate migration to add extra columns to the user model
$    rails generate migration add_username_to_users username:string:uniq

#    generate a likes model
$    rails generate model like like_count:integer post:references

#    generate a posts model
* rails generate model post caption:string user:references image:string

Now that we have generated our migration files, let's make some modifications to some files before running our migrations.

Update the code in your application controller, with the following:

# app/controllers/application_controller.rb

class ApplicationController < ActionController::Base
  protect_from_forgery with: :exception
  before_action :configure_permitted_parameters, if: :devise_controller?
  before_action :authenticate_user!

  protected

  def configure_permitted_parameters
    added_attrs = [:username, :email, :password, :password_confirmation, :remember_me]
    devise_parameter_sanitizer.permit :sign_up, keys: added_attrs
    devise_parameter_sanitizer.permit :account_update, keys: added_attrs
  end
end

Also, update the code in your likes migration file with the following

# db/migrate/20180524215616_create_likes.rb

class CreateLikes < ActiveRecord::Migration[5.1]
  def change
    create_table :likes do |t|
      t.integer :like_count, default: 0
      t.references :post, foreign_key: true

      t.timestamps
    end
  end
end

Now, we’re ready to run our migration and see our app. In your terminal, run the following:

#    run database migrations
$    rails db:migrate

* After running migrations, start the development server on your terminal by running rails s. Visit http://localhost:3000 in your browser to see your brand new application:

### Styling the pages
To style our pages, we'll make use of Bootstrap. We have already added Bootstrap to our app while setting it up, so all we need to do is require it and add our markup and styles.
First we'll generate a controller for our posts; in your terminal, run the following command:

#    generate a posts controller
$    rails g controller posts

* Update the code in the following files with the code in each link provided below:

Replace the code in app/assets/javascripts/application.js with the code above:

        // This is a manifest file that'll be compiled into application.js, which will include all the files
        // listed below.
        //
        // Any JavaScript/Coffee file within this directory, lib/assets/javascripts, or any plugin's
        // vendor/assets/javascripts directory can be referenced here using a relative path.
        //
        // It's not advisable to add code directly here, but if you do, it'll appear at the bottom of the
        // compiled file. JavaScript code in this file should be added after the last require_* statement.
        //
        // Read Sprockets README (https://github.com/rails/sprockets#sprockets-directives) for details
        // about supported directives.
        //
        //= require rails-ujs
        //= require turbolinks
        //= require popper
        //= require jquery3
        //= require bootstrap
        //= require_tree .


Replace the code in app/views/devise/registrations/new.html.erb with the code here
Replace the code in app/views/devise/sessions/new.html.erb with the code 

    <div class="container-fluid bg-dark full-height d-flex justify-content-center align-items-center">
        <div class="container bg-light col-lg-5 auth-container">
        <h3 class="text-center">InstaClone</h3>
        <h5 class="text-center">Log in</h5>

        <%= form_for(resource, as: resource_name, url: session_path(resource_name)) do |f| %>
        <div class="field">
            <%= f.label :email %><br />
            <%= f.email_field :email, autofocus: true, autocomplete: "email", required: true, class: 'form-control' %>
        </div>

        <div class="field">
            <%= f.label :password %><br />
            <%= f.password_field :password, autocomplete: "off", required: true, class: 'form-control' %>
        </div>

        <% if devise_mapping.rememberable? -%>
            <div class="field">
            <%= f.check_box :remember_me %>
            <%= f.label :remember_me %>
            </div>
        <% end -%>

        <div class="actions">
            <%= f.submit "Log in", class: 'btn btn-primary my-2' %>
        </div>
        <% end %>

        <%= render "devise/shared/links" %>
    </div>
    </div>
Rename your app/assets/stylesheets/application.css file to app/assets/stylesheets/application.scss and replace the code there with the code 

            /*
            * This is a manifest file that'll be compiled into application.css, which will include all the files
            * listed below.
            *
            * Any CSS and SCSS file within this directory, lib/assets/stylesheets, or any plugin's
            * vendor/assets/stylesheets directory can be referenced here using a relative path.
            *
            * You're free to add application-wide styles to this file and they'll appear at the bottom of the
            * compiled file so the styles you add here take precedence over styles defined in any other CSS/SCSS
            * files in this directory. Styles in this file should be added after the last require_* statement.
            * It is generally better to create a new file per style scope.
            *
            *= require_tree .
            *= require_self
            */

            @import "bootstrap";
            @import url('https://fonts.googleapis.com/css?family=Dosis');

            body {
            font-family: 'Dosis', sans-serif;
            }

            .full-height {
            height: 100vh;
            }

            .auth-container {
            border-radius: 3px;
            padding: 1rem;
            }

            .alert-container {
            position: fixed;
            }

            .post-wrapper {
            margin: 1rem auto;
            }

            .post {
            // min-height: 30rem;
            max-height: 30rem;
            overflow: hidden;
            }

            .post-user {
            font-weight: 700;
            }

            .like-icon {
            cursor: pointer;
            text-decoration: none !important;
            }

            .pagination {
            font-size: 12px;
            }

            .pagination > * {
            margin-right: 0.1em;
            padding: 0.3em 0.4em;
            }

            .pagination a:hover {
            background: #202020 none repeat scroll 0 0;
            text-shadow: 1px 1px 1px #171717;
            }

            .pagination a:active {
            text-shadow: none;
            }

            .pagination .current {
            background: #202020 none repeat scroll 0 0;
            color: white;
            text-shadow: 1px 1px 1px #171717;
            }

            .pagination .disabled {
            color: #C0C0C0;
            }


Add the code here to your app/controllers/posts_controller.rb file.

        class PostsController < ApplicationController
            def index
            @posts = Post.all.order(created_at: :desc).paginate(page: params[:page], per_page: 5)
            end
        
            def new
            @post = Post.new
            end
        
            def create
            @user = User.find(current_user.id)
            @post = @user.posts.new(post_params)
            @post.likes.build()
            
            respond_to do |format|
                if @post.save
                format.json { render :show, status: :created }
                else
                format.json { render json: @post.errors, status: :unprocessable_entity }
                end
            end
            end
        
            def show
            @post = Post.find(params[:id])
            end
        
            def add_like
            @post = Post.find(params[:post_id])
            if @post
                @post.likes[0].like_count +=1
        
                if @post.likes[0].save
                respond_to do |format|
                    format.json { render :show, status: :ok }
                end
                end
            end
            end
        
            private
            def post_params
                params.permit(:caption, :image)
            end
        end


Add the code here to your config/routes.rb file.
If we reload our app in the browser, we should be greeted with a lovely sight. Go ahead and create an account.

If you encounter any error related to application.html.erb, in config/boot.rb, change the ExecJS runtime from Duktape to Node.
* Deployment instructions

* ...


### Pusher account setup
Now that our application is up and running, head over to [Pusher](https://dashboard.pusher.com/accounts/sign_up) and sign up for a free account.
After creating a Pusher account, create a new app by selecting Channels apps on the sidebar and clicking Create Channels app button on the bottom of the sidebar.
Configure your app by providing basic information requested in the form presented. You can also choose the environment you intend to integrate with Pusher, to be provided with some boilerplate setup code. After that, click the App Keys tab to retrieve your keys.