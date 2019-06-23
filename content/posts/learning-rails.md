---
title: "Learning Web Development with Rails"
description: "Notes on starting to learn rails"
date: "2014-05-05"
categories: 
    - "Rails"
    - "Ruby"
    - "Web"
---

Here are some of my notes from learning rails. I am following the course [Web Application Architectures](https://class.coursera.org/webapplications-001) at [Coursera](https://www.coursera.org). This is seen from perspective of someone used to more desktop applications. I used Ruby as a scripting language for writing small unix tools in the past, but I am not considering myself very up to date.

# Setup Project

Have rails autogenerate a bunch of files for a project:

	$ rails new myproject

This seems to be the way to create a new model class with corresponding table in database:

	$ rails generate scaffold comment post_id:integer body:text
	
This will to a whole bunch of things. A **scaffold** from what I can understand is a collection of related:

* Database table
* Model class
* Controller class
* HTML template files representing view


So I will get a table for my comment, a definition of a model class. Actually the model class won't contain anything, but rails creates methods on it at runtime for me to access variables. That is part of `ActiveRecord` from what I understand. One can call methods such as:

	a_comment     = Comment.new
	all_comments  = Comment.all
	first_comment = Comment.first
	
and this will cause SQL statements to be formed fetching the data out from the underlying table.

	> Comment.first
		Comment Load (0.1ms)  
		SELECT  "comments".* FROM "comments"   
		ORDER BY "comments"."id" ASC LIMIT 1
		=> #<Comment 
			id: 2, 
			post_id: 1, 
			body: "This is my first comment", 
			created_at: "2014-05-05 10:15:52", 
			updated_at: "2014-05-05 10:15:52">

I've taken the liberty to format the output for readability. The Controller objects are apparently supposed to be mediators like in Cocoa, but it seems to be used a bit different. They seem to provide the actions the user perform on an object from the user interface. We can use the `rake routes` to find out what REST services are provided. Or probably this isn't called REST but is just a bunch of URLs for performing different HTTP actions. But it seems to correspond to what is created in the controllers.

	$ rake routes
           Prefix Verb   URI Pattern                                 Controller#Action
         comments GET    /comments(.:format)                         comments#index
                  POST   /comments(.:format)                         comments#create
      new_comment GET    /comments/new(.:format)                     comments#new
     edit_comment GET    /comments/:id/edit(.:format)                comments#edit
          comment GET    /comments/:id(.:format)                     comments#show
                  PATCH  /comments/:id(.:format)                     comments#update
                  PUT    /comments/:id(.:format)                     comments#update
                  DELETE /comments/:id(.:format)                     comments#destroy
                  
# Inspecting and Debugging

You can interact with your whole rails app from the command line. You don't need a browser running just run:

	$ rails console

Then you can go ahead and call methods such as:

	a_comment     = Comment.new
	all_comments  = Comment.all
	first_comment = Comment.first

mentioned earlier. I recommend using `pry` it is much better than the standard `irb` which is a bit antiquated. Install with:

	$ gem install pry pry-doc
	
Then you can run the rails console with:

	$ pry -r ./config/environment.rb
	
`pry` let you do some neat things like `cd` and `ls` into classes or objects. 

	> cd 42.6
	1> round
	=> 42
	1> exit
	>

Great way to drill down into your objects and inspect them.  `pry` wans't covered in the course I just found that online.
	
# Databases and SQL

You don't setup the tables in the database yourself directly. Instead rails will generate a `schema.rb` file. Looking like:

	ActiveRecord::Schema.define(version: 20140502112529) do
	
	  create_table "comments", force: true do |t|
	    t.integer  "post_id"
	    t.text     "body"
	    t.datetime "created_at"
	    t.datetime "updated_at"
	  end
	end
	
This ruby code will generate all the necessary SQL statements to actually create the database.