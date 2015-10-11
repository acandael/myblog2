---
title: From Web Integrator to Web Programmer with Ruby on Rails
date: 2014-12-27 16:25 UTC
tags:
---


<span style="float:right;">
![ruby on rails logo](/images/ruby_on_rails.svg.png)
</span>

For the redesign of the cosmetologist website [www.anniek-lambrecht.be](http://www.anniek-lambrecht.be) I decided
to rebuild the website using the [Ruby on Rails](http://rubyonrails.org/) web framework.

For the content management of my websites, I used to integrate an open source
framework like Drupal, Wordpress or Umbraco, but lately I have shifted this
development strategy and prefer developing the content management functionality
myself. Let me tell you why.


## Why I shifted my focus to webdevelopment with Rails

For the last two years I have evolved from a 'web integrator' integrating
my web solutions in a content management system framework to a 'web programmer' building my solutions from scratch. 

Using an open source content management definitly speeds up the development process, but to me it became more and more a hindrance building my solutions around prebuild code. As a web developer, I want to build my web applications from scratch, so I have full control over my code base and I can change and extend the solution as I see
fit.

Rails seemed like the perfect framework to realise my development goals. It gives you absolute freedom in what you build, but some best programming practices like separation of concerns and testing are baked right into the framework. 

It sure took me a while to become a proficient Rails developer. Not only
did I need to conquer my fear of the command line, I needed to learn a new
programming language, [Ruby](https://www.ruby-lang.org/en/), and a whole new stack of tools like PostgreSql, Capistrano, Rspec and Capybara. To deploy my Rails applications I needed to get a certain level of proficiency with Linux and Unix.

Now I'm glad I made the jump. Being able to build anything from the ground up, and not depend on a CMS framework to realize a technical solution gives an immense sense of freedom. In the process of learning these new tools and programming language I became a better web developer.

##An agile development approach##

Another reason why I wanted to be less dependent on a CMS framework is that I noticed that the majority of websites I integrated into a full fledged CMS framework, only update limited portions of their content.

So instead of building fully editable websites upfront, I chose a more agile
approach, where I only implement content management functionality for the
content that needs updating on a regularly basis. Content that is not presumed
to change a lot over time, I leave static. If, later on, the application
needs more content management functionality, it's easy to extend the
application because I have a well architected code base that is backed by an extensive test suite.

The result of this agile approach are web applications with trimmed down content management functionality which makes it really straightforward for the owner of the website to maintain and update its content. 

Applied to the cosmetologist web application this means a solution that
enables the editor to add and edit treatments in a straightforward manner. A
nice bonus of this custom development is that the content management
functionality is fully integrated in the styling of the web application. The
administrator just needs to log in, click the 'admin' bottom and start editing.
He can check his work right away by clicking on the page link in the navigation
menubar.


![cms funtionality for updating treatments](/images/dashboard.jpg)
*CMS functionality for updating treatments*


As a web developer I've come a long way. From a 'web integrator', integrating
solutions into a content management framework, to a 'web programmer',
building web solutions from scratch using Rails to build well architected
solutions that are easy to change and extend.






