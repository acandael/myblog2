---
title: Middleman - automate your data with the data folder and partials
date: 2015-11-21 15:20 UTC
tags:
---

Lately static website generators are booming. I won't talk here about why and how of static website generators, for that I refer to the excellent article by Mathias Biilmann, [Why Static Website Generators Are The Next Big Thing](http://www.smashingmagazine.com/2015/11/modern-static-website-generators-next-big-thing/). I can also refer to this [review of some static site generators](http://www.smashingmagazine.com/2015/11/static-website-generators-jekyll-middleman-roots-hugo-review/) by the same author. I love Middleman because it gives me the power of Ruby on Rails without the deployment headaches.

In this article I want to show you how I cleaned up static html by the use of partials and the data folder. The **data** folder is Middleman's replacement for a database. You can store data in this folder as .yml files.

On my personal website I have a projects page where I list the software projects I've been working on. The html looks like this:

    source/projects.html.erb

    <article>
      <div class="screenshot">
        <img src="images/homepage_actionmaker.jpg", alt: "screenshot homepage ActionMaker"/>
      </div>
      <div class="description">
        <header>
          <h1>Events agency ActionMaker</h1>
          <span class="website"><span class="website"><i class="fa fa-link"></i><a href="http://www.actionmaker.be">www.actionmaker.be</a></span>
          </header>
          <h2>Features</h2>
          <ul class="features">
            <li>Responsive images</li>
            <li>Responsive event slider and image gallery</li>
            <li>Can be viewed on smartphones, tablets, desktops and laptops</li>
          </ul>
          <p>Event agency organizing teambuilding events and bachelor parties. What better way than to present teambuilding with photo's, lot's of photo's. Each teambuilding activity is presented with a responsive image gallery and a contact form.</p>
      </div>
    </article>
    <article>
      ...
    </article>
    ...

As you can see, this is quite repetitive code. As each project is wrapped in an article tag, it's and ideal candidate to replace by a partial. To clean it up, I dropped the data in a projects.yml file in de data folder:

    data/projects.yml

    - name: Events agency ActionMaker

      image: homepage_actionmaker.jpg

      website: http://www.actionmaker.be 

      features:
        - Responsive images
        - Responsive event slider and image gallery
        - Can be viewed on smartphones, tablets, desktops and laptop

      description: Event agency organizing teambuilding events and bachelor parties. What better way than to present teambuilding with photo's, lot's of photo's. Each teambuilding activity is presented with a responsive image gallery and a contact form.
        
        ...

For those who aren't familiar with the .yml format, you structure your data by indentation and dashes.

Now on the projects page I can loop through my data like this:

    source/projects.html.erb

    <% data.projects.each do |project| %>
      <%= partial "project", locals: { project: project } %>
    <% end %>

In the project partial I inject the data by accessing the data through the local project object that was passed through to the partial:

    source/_project.erb

    <article>
      <div class="screenshot">
        <%= image_tag project["image"] %>
      </div>
      <div class="description">
        <header>
          <h1><%= project["name"] %></h1>
          <span class="website"><span class="website"><i class="fa fa-link"></i><%= link_to project["website"] %></span>
        </header>
        <h2>Features</h2>
        <ul class="features">
          <% project["features"].each do |feature| %>
            <li><%= feature %></li>
          <% end %>
        </ul>
        <p><%= project["description"] %></p>
      </div>
    </article>
    
As you can see, this looks very familiar to the code in a Ruby on Rails project, with the only difference that the data is not coming from a database but from a local file.

The projects page is now a lot easier to maintain. To add a new project to the page, I just need a to enter a new entry in the projects.yml file.

If you are a Ruby on Rails developer, and you are looking for a static solution, I greatly encourage you to take a look at Middleman.


