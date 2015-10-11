---
title: Building hand-crafted websites with Middleman
date: 2014-05-29 17:53 UTC
---

For the redesign of my personal website, I wanted to have a small website, with
a couple of static pages, a blog and contact page.

I had already played with [Octopress](http://octopress.org/), which is a blogging engine build on top of
the static site generator [Jekyll](http://jekyllrb.com/), and I liked the fact that blog posts can be
stored as markdown files.

However I was looking for a static site generator that would play nice with my
favourite front end tool [Bourbon](http://bourbon.io/).

[Middleman](http://middlemanapp.com/) is a lightweight static site framework built using Ruby. It compiles Markdown into HTML and is easily deployed to S3, Heroku, or GitHub Pages

In the next sections I will explain how I set up my personal website with a
customized blog system. For that I used the very helpful blogpost [Styling a Middleman Blog with Bourbon, Neat, and Bitters](http://robots.thoughtbot.com/middleman-bourbon-walkthrough). Also feel free to checkout my [repository](https://github.com/acandael/personal-site) for this website that was build with Middleman.


##Setting up Middleman

      gem install middleman

##Create your project

      middleman init my_project

A brand new project is created with a source folder where most of your
content will live, and a config.rb file for Middleman settings. Next we'll add
the blog pages.


##Building the Blog

First we need to add the middleman-blog gem into our Gemfile

**Gemfile**

      gem "middleman-blog"

In the config.rb file we activate the blog

      activate :blog do |blog|
      # set options on blog
      end

Now we just need to initialize the blog with the blog template option to
generate the sample index.html, tag.html, calendar.html, and feed.xml files

      middleman init --template=blog


Now we can start the server and see what we got

      cd my_blog
      $ bundle exec middleman
      $ open http://localhost:4567


##Use Bourbon
In the Gemfile add

      gem 'bitters'
      gem 'bourbon'
      gem 'neat'

Bourbon is a Sass mixin library that's very lightweight, Neat is a semantic
grid framework build on top of Bourbon. Bitters provide default settings and a
Sass project structure. You can read more about Bourbon in my previous blogpost
['Creating Responsive Websites with Bourbon Neat'](/blog/2014/05/24/creating-responsive-websites-with-bourbon-neat/)

Since we updated our Gemfile we need to bundle and restart the server

      bundle
      bundle exec middleman

Bitters needs an additional install in your stylesheets folder

      $ cd source/stylesheets
      $ bitters install
      Bitters files installed to /bitters

##Add some styling
To add some styling we create a stylesheet manifest with the correct character encoding and import our design libraries:

**source/stylesheets/all.css.scss**

      @charset "utf-8";

      @import "bourbon";
      @import "bitters/bitters";   /* Bitters needs to be imported before Neat */
      @import "neat";

Include all of the stylesheets to be used in this site by adding the link to the manifest in the head section of layout.erb:

**source/layout.erb**

      ...
      <head>
        <%= stylesheet_link_tag "all" %>
      ...

Next I create a partial to contain all my layout settings:

**source/stylesheets/partials/_layout.scss**

      ...
      section {
        @include outer-container;
      }
      ...

The outer-container() mixin will center all section elements in the viewport

Don't forget to import the layout partial into your manifest

**source/stylesheets/all.css.scss**

      ...
      @import "partials/layout";

To have a good reading experience, we want to have a comforable length for the
lines of text.

Neat makes this easy to accomplish. To import the gridsettings into Bitters we just uncomment that line in
_bitters.scss

      ...
      @import "grid-settings";

Then edit the _grid-settings.scss partial. Uncomment the $max-width variable and change its value to em(700). This should give us a comfortable line-length for reading.

      ...
      $max-width: em(700);
      ...

##Creating a nested layout for the blogposts
Although the middleman blog template comes with a blog listing template, it
comes not with a template for blog posts.
For this purpose we'll create a nested layout. Create a 
blogpost.erb layout file and put it in the source/layouts folder
**source/layouts/layout.erb**

      <% wrap_layout :layout do %>
        <article>
          <%= yield %>
        </article>
      <% end %>

Like a normal layout, yield is where the resulting template content is placed.


##Syntax hightlighting
A feature I needed for my blog posts was syntax highlighting. Middleman-syntax
is an extension that adds syntax highlighting via [Rouge](https://github.com/jneen/rouge).

**Gemfile**

      ...
      gem 'middleman-syntax'

then run bundle install and activate it in config.be

      activate :syntax

because I want syntax-highlighting for Markdown files, I add the following
configurations to my config.be

      set :markdown_engine, :kramdown

      set :markdown, :layout_engine => :erb,
                     :fenced_code_blocks => true,
                     :tables => true,
                     :autolink => true,
                     :smartypants => true,
                     :with_toc_data => true
      

The last thing we need to do is create the highlighting css and add it to our
manifest
**highlight.css.erb**

      <%= Rouge::Themes::Github.render(:scope => '.highlight') %>

**source/stylesheets/all.css.scss**

      ...
      @import 'highlight';

##Generating articles##
We're pretty much done now and can start writing our blogpost. The
middleman-blog gem has an easy to use blogpost generator. In the terminal you
can create a blogpost with the middleman article Title command:

      $middleman article "my interesting new article"


This creates a file 2014-05-31-my-interesting-new-article.html.markdown,
with some [Frontmatter](http://middlemanapp.com/basics/frontmatter/) set up:

      ---
      title: my interesting new article
      date: 2014-05-31 08:30 UTC
      tags:
      ---
      
##Conclusion

Middleman is a very powerfull static site generator, build on Ruby, that is ideal for small
websites. It provides templating and shared layouts and brings back joy to
building hand-crafted websites. Because Middleman projects build completely static files they are easy to
deploy to S3, Heroku, or GitHub Pages.

For further instruction on how to setup and use a Middleman project, checkout
there very well managed [documentation](http://middlemanapp.com/basics/getting-started/).




