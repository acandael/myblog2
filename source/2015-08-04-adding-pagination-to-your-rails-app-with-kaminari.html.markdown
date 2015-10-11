---
title: Adding Pagination To Your Rails App with Kaminari
date: 2015-08-04 12:52 UTC
tags: Rails
---

For a Rails web application I needed to implement pagination. I know some resources on the web that instruct on how to implement pagination using the [will-paginate](https://github.com/mislav/will_paginate) gem. So I checked the will-paginate gem on Ruby Toolbox. However, there I noticed a comment that worried me:

>“You should really use Kaminari instead, will-paginate does all kind of scary things causing very hard to follow errors.”

Also on the Railscast video on implementing [pagination with Kaminari](http://railscasts.com/episodes/254-pagination-with-kaminari), I heard a same comment by Ryan Bates:

>“I think it’s a much cleaner implementation than will-paginate”

So I checked the [Kaminari gem](https://github.com/amatsuda/kaminari), and found out it’s pretty easy to implement.

## Get started

put the gem in your Gemfile:

    gem 'kaminari'

then run bundle

    % bundle

## Implementing Pagination in the Controller

At the controller-level you need to call the page(id) method on your model:

    def index
      @articles = Article.page(params[:page])
    end

By default Kaminari will load 25 records per page, but you can set it yourself with de per(number) method.
Say you would like to show 10 records per page you could change your query like so:

    def index
      @articles = Article.page(params[:page]).per(10)
    end


## Implementing Pagination in the View

Then all that's left to do is call the paginate helper in our view:

    <%= paginate @articles %>

That's all it takes to implement pagination. Easy, isn't it?

## Customizing the View

The default view template for the Kaminari paginator will wrap the page links in span-elements. However, I'm using
the [Bourbon component](http://refills.bourbon.io/components/) for styling a paginator. Here the page links are wrapped in li-elements. This is no problem
as the Kaminari gem has a template generator to customize your view template. Run the generator command in the terminal:

    % rails g kaminari:views default

Then, under app/views/kaminari/ you can find the Kaminari templates. To give you an example, this is how my paginator.html.erb template looks:

    <%= paginator.render do -%>
    <nav class="pagination">
      <ul>
        <%= prev_page_tag unless current_page.first? %>
        <li>
          <ul>
          <%= first_page_tag unless current_page.first? %>
          <% each_page do |page| -%>
          <% if page.left_outer? || page.right_outer? || page.inside_window? -%>
          <%= page_tag page %>
          <% elsif !page.was_truncated? -%>
          <%= gap_tag %>
          <% end -%>
          <% end %>
          </ul>
        </li>
        <%= next_page_tag unless current_page.last? %>
        <%= last_page_tag unless current_page.last? %>
      </ul>
    </nav>
    <% end -%> 


And this is how my pagination component looks in the browser:

![Pagination with Kaminari](/images/pagination.jpg)

Actually most of my time was spent in customizing the view templates. If you don't want to invest your time in customizing the view templates, there also a posibility to fetch [Kaminari themes](https://github.com/amatsuda/kaminari_themes).

