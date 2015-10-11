---
title: creating responsive websites with Bourbon Neat
date: 2014-05-24 19:25 UTC
author: "Anthony Candaele"
tags: "responsive"
---

For my latest web project, a small retail website for a cosmetologist, I
decided to use the semantic grid framework [Bourbon Neat](http://neat.bourbon.io/)
<!--more-->
The brief for this website was that visitors should easily be able to order a beauty treatment provided by the cosmetologist. The website should be responsive and easy to use on a broad collection of devices like smartphones, tablet-pc’s, laptops and desktops.

The end result of all this is a website that is easily consulted and used on smartphones, tablet-pc’s, laptops and desktops:


![My helpful screenshot](/images/all_devices.jpg)

##Why Bourbon Neat?##

The reason I chose this framework over popular frameworks like Bootstrap or Foundation are that Neat is a semantic grid framework. This means that my html markup doesn’t get cluttered with non semantic div’s for defining the grid.

So instead of implementing the grid layout into my html markup:


      <div class=“row”>
        <section>
          <aside class=“col-md-3”>
            …
          </aside>
          <article class=“col-md-9”>
            ...
          </article>
        </section>
      </div>



bourbon Neat lets me implement the grid layout through Css:

      section {
        @include outer-container;
        aside { @include span-columns(3); }
        article { @include span-columns(9); }
      }

This makes my html markup stay clean and semantic:

      <section>
        <aside>What is it about?</aside>
        <article>Neat is an open source semantic grid framework built on top of Sass and Bourbon…</article>
      </section>

##Using Sass##

Neat is a grid framework build upon Bourbon, a lightweight mixing library for Sass. So to use this frameworks, you have to be comfortable with Sass.

As you may know, [Sass](http://sass-lang.com/) stands for Syntactically Awesome Stylesheets, and is what we call a Css preprocessor and provides a syntax that allows the use of variables, nesting of styles, mixing and other cool stuff. Overal Sass stands for better management of your Css code. I won’t go into Sass here, but I can refer to this excellent blogpost by Dan Cederholm: [Why Sass](http://alistapart.com/article/why-sass).

I didn’t have much difficulties getting used to the Sass language, but organising the file structure for my stylesheets was a bit overwhelming at first.

After checking out this great resource: [how to structure a sass project](http://thesassway.com/beginner/how-to-structure-a-sass-project), I came up with this structure:

      project
      - stylesheets/
        all.css
        _all.scss
        -- application/
            _index.scss
            _contact.scss
            …
        -- base/
           _base.scss
           _extensions.scss
           _forms.scss
           _typography.scss
           _variables.scss
        -- bitters/
        -- bourbon/
        -- neat/
 
the Sass code gets compiled into all.scss

The applications folder contains a .scss file for every page in the project (eg. _index.scss, _contact.scss, …)
 
The ‘base’ folder contains common variables for typography, colours, extensions, etc …
    
The 'bitters' folder contains styles for all the basic elements used troughout
the site's style. It also contains folders for custom mixins and extends for your site as well.

##Using Bourbon mixins##

[Bourbon](http://bourbon.io/), is a mixing library for Sass and can be compared with [Compass](http://compass-style.org/), but is more simple and lightweight. Another advantage of Bourbon, is that it automatically compiles to updated vendor prefixes.

For instance:

      section {
        @include linear-gradient(to top, red, orange);
      }

compiles to:

      section {
        background-color: red;
        background-image: -webkit-linear-gradient(bottom, red, orange);
        background-image:         linear-gradient(to top, red, orange);
      }

##Using Media Query Mixins##

Also very handy since Sass 3.2 is the use of mixins for media queries. Instead of dumping all your styles in a single media query definition, you can now use media query mixins where you see fit.

For instance, for the section element on the homepage I defined this style definitions for the desktop, with additional styles for mobile and tablet using media query mixins:

      section.content {
        @include outer-container;
        margin-top: 30px;
        /* media query mixin for mobile screens */
         @include media($mobile) {
          margin-top: 0px;
        }
        /* media query mixin for tablet-pc screens */
        @include media($tablet) {
          margin-top: em(5);
        }
      }

    

Sass happily translates this to:

       section.content {
        *zoom: 1;
        max-width: 68em;
        margin-left: auto;
        margin-right: auto;
        margin-top: 30px; }
        section.content:before, section.content:after {
          content: " ";
          display: table; }
        section.content:after {
          clear: both; }
         @media screen and (max-width: 480px) {
          section.content {
            margin-top: 0px; } }
        @media screen and (max-width: 768px) {
          section.content {
            margin-top: 0.3125em; } }


##Some IE8 issues##

In the testing phase, it seemed that there where some issues with older browser. More precisely, IE8 had some layout problems. Seems this had to do with IE8 not understanding some CSS3 selectors. Luckily there’s a solution for this with the Javascript framework called [Selectivizr](http://selectivizr.com/). All you need to do is define, between conditional tags, a reference to the Selectivizr script:

      <!--[if lte IE 8]>
         <script type="text/javascript" src="selectivizr.js"></script>
      <![endif]-->

Selectivizr use jQuery, so make sure define a reference to jQuery before you reference the Selectivizr script.

Another issue I ran into was the html5 placeholder tag wich is not recognised by older browsers. There are some solutions for this. The one I use is the excellent [Html5 Placeholder jQuery Plugin](https://github.com/mathiasbynens/jquery-placeholder) by Matthias Bynens 

Using it is very easy, just reference the placeholder.js script and put this one line of jQuery in your page:

      $('input, textarea’).placeholder();


##Conclusion##

I’m a developer that likes to have control over his codebase. Having full control over what gets in to my code was a main motivation for choosing a lightweight framework like Bourbon Neat. Another reason for choosing this grid framework is that it's a semantic grid framework resulting in clean semantic code, not polluted with numerous div elements just for defining the grid layout.

I much encourage the reader to check out Bourbon Neat, which was developed by [Thoughtbot](http://www.thoughtbot.com/). Lately they added [Bitters](http://bitters.bourbon.io/) (Scaffold styles, variables and structure for Bourbon projects) and [Refills](http://refills.bourbon.io/) (prepackaged patterns and components, built on top of Bourbon, Bitters, and Neat).


