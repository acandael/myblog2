---
title: Easy file uploads in Rails with Refile
date: 2015-08-20 08:56 UTC
tags: Rails
---

A common task when developing Rails web applications is to implement file uploads.

Two popular gems used for uploading files are [Paperclip](https://github.com/thoughtbot/paperclip) and [Carrierwave](https://github.com/carrierwaveuploader/carrierwave).

Recently I found out about this new gem [Refile](https://github.com/refile/refile) which makes implementing file
uploads in Rails a breeze.

It’s Jonas Nicklas third take on a file upload library and I think he has done a terrific job.

One of the great features of Refile is it’s simplicity. While Paperclip and Carrierwave are quit involved to implement, file uploads with Refile is as simple as adding an image column to your database, adding an image upload attribute to your model, and using a form helper to render the file upload field in a form.

Another great feature of Refile is it’s Javascript implementation. You can for instance implement direct image uploads before a user clicks the submit button, by adding the ‘direct’ attribute to your form helper:

  <%= form.attachment_field :image, direct: true %>

Make sure to add a require statement in your manifest file:

    your-app/app/assets/javascripts/application.js
    ...
    //= require refile
    //= require_tree .


In the next sections I’ll walk you to the steps to implement file uploads with Refile to your Rails application. The sample code will implement image file uploads for news articles.

## Get Started ##

Add the Refile gem to your Gemfile

    gem "refile", require: "refile/rails"
    gem "refile-mini_magick"

The refile-mini_magick gem is for image processing. Make sure you have
[ImageMagick](http://imagemagick.org/script/index.php)
installed on your system.

The next step is to add the image attribute to our NewsArticle model:

    class NewsArticle << ActiveRecord::Base
    ...
      attachement :image, type: :image
    end

The type: :image option ensures the image file extension is .jpg, .gif or
.png

The next step is to create a migration for our file upload:

    rails generate migration add_image_to_news_articles image_id:string,
    image_content_type:string
    rake db:migrate

Now we need to add the image attachment field to our form:

    <%= form_for @news_article]} do |f| %>
    ...
      <% if @news_article.image_id? %>
        <%= image_tag attachment_url(@news_article, :image, :fill, 150, 150) %>
        <div class="check_box">
          <%= f.check_box :remove_image %>
          <%= f.label :remove_image %>
        </div>
      <% end %>
      <%= f.attachment_field :image, direct: true, data: { remote_id: :news_article_image_cache_id } %>
    ...
    <% end %>

The image upload field is provided with the f.attachment_field form helper. The
direct: true option makes sure the image gets uploaded in the background before
the user clicks the submit button.

The :remove_image form field adds a checkbox to the form, so the user can check
the image for removal.

I'm also using a [custom Coffeescript file](https://github.com/acandael/hms-rails/blob/master/app/assets/javascripts/custom_refile.js.coffee) to dynamically load the image from the cache into the form, before the user clicks the submit button.

The resulting form looks like this:
![Image upload form](/images/image_in_form.jpg)


Now we need to make sure the :image and :remove_image properties are added to
the strong parameters list in the news_articles controller:

    class NewsArticlesController < ApplicationController
    ...
    private
      def news_article_params
        params.required(:news_article).permit(:image, :image_cache_id, :remove_image)
      end
    ...
    end

We are almost finished, the only thing left to do is show the image in our
view:

    <%= image_tag attachment_url(@article, :image, :fill, 250, 250) %>

Our uploaded image is rendered in the view, it has a dimension of 250 pixels by
250 pixels.

![The image rendered in the view](/images/image_on_page.jpg)

The Refile gem can store our file uploads on AWS. The initializer file is pretty straightforward:

    your_app/config/initializers/refile.rb

    require "refile/backend/s3"

    if Rails.env.production?
      aws = {
        access_key_id: ENV["AWS_ACCESS_KEY_ID"], 
        secret_access_key: ENV["AWS_SECRET_ACCESS_KEY"], 
        bucket: "name of your bucket"
      }

      Refile.cache = Refile::Backend::S3.new(prefix: "cache", **aws)
      Refile.store = Refile::Backend::S3.new(prefix: "store", **aws)
    end


Of course the Refile gem holds lots of other options and configurations. Make
sure to check out the [Refile API documentation](http://www.rubydoc.info/gems/refile). Also check out this excellent [video on Refile](https://gorails.com/episodes/file-uploads-with-refile) by Chris Oliver

