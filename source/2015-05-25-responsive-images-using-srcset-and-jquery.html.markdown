---
title: Responsive Images using srcset and jQuery
date: 2015-05-25 17:55 UTC
tags: responsive, jquery
---

For my latest web project, [Action Maker](http://superintendant-rabbit-82576.bitballoon.com/), an event agency, I needed to build a responsive website with a heavy focus on imagery. This event agency organizes teambuilding events. And what better way to promote teambuilding than with images, lot’s of images. The challenge with this project was to make these images load fast on mobile devices. Another challenge was to present the image sliders and image galleries in a responsive way.

In the following I’ll explain the techniques and libraries I used to solve these technical challenges.

## Responsive images with the srcset attribute

To make the images more responsive I used the ‘srcset’ attribute on the image element. With the srcset attribute, the browser can intelligently choose the best image for a given screen width. An example for an image element with the srcset attribute set is:

    <img srcset="images/banner-large.jpg 2850w,
	    images/banner-medium.jpg 1425w,
	    images/banner-small.jpg 700w"
	    src="images/banner-medium.jpg" alt="schermen" />

The above markup tells the browser to download the banner-large.jpg image file for screens with a screen width of at least 2850 pixels. For screens with a screen width of at least 1425 pixels it will download the banner-medium.jpg file. At last, for  screens with a screen width not larger then 700 pixels it will download the banner-small.jpg file.

In this example I used a width descriptor ‘w’ to let the browser intelligently choose the right image, based on the screen width. However you can also use a  pixel density descriptor ‘x’ to enable the browser to intelligently choose an image file based on the pixel density of the device.

For more information see the [srcset documentation](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/img#attr-srcset) on the Mozilla Developer Network.

##jQuery libraries for responsive sliders and image galleries

For this project I also made extensive use of some jQuery libraries for an easy image browsing experience.

The first library is called [bxSlider](http://bxslider.com/), it’s a responsive jQuery content slider that is very easy to implement. 

![bxSlider, a responsive image slider](/images/slider-image.jpg)

Just link to the Javascript file and implement the banner in your markup like this:

     <ul class="bxslider">
       <li><img srcset="images/banner-large.jpg 2850w,
		     images/banner-medium.jpg 1425w,
		     images/banner-small.jpg 700w"
		     src="images/banner-medium.jpg" alt="schermen" />
       <li>
       <li><img srcset="images/banner-boogschieten-large.jpg 2850w,
		     images/banner-boogschieten-medium.jpg 1425w,
		     images/banner-boogschieten-small.jpg 700w"
		     src="images/banner-boogschieten-medium.jpg" alt="boogschieten" />
       </li>
       <li><img srcset="images/banner-kleiduifschieten-large.jpg 2850w,
		     images/banner-kleiduifschieten-medium.jpg 1425w,
		     images/banner-kleiduifschieten-small.jpg 700w"
		     src="images/banner-kleiduifschieten-medium.jpg" alt="kleiduifschieten" />
       </li>
 
In this example I’m also using the srcset attribute as described in the previous section.

For the image gallery, I’m using a jQuery library called [Magnific Popup](http://dimsemenov.com/plugins/magnific-popup/). 
![Magnific, a responsive lightbox](/images/image-lightbox.jpg)

It’s a responsive lightbox and dialog script that’s just like bxSlider very straightforward to implement. Just add your images to an unordered list:

    <section class="gallery">
      <ul>
        <li><a href="/images/boogschieten-1-big.jpg"><img
            class="boogschieten-1" /></a></li>
        <li><a href="/images/boogschieten-2-big.jpg"><img class="boogschieten-2" /></a></li>
        <li><a href="/images/boogschieten-3-big.jpg"><img class="boogschieten-3" /></a></li>
        <li><a href="/images/boogschieten-4-big.jpg"><img class="boogschieten-4" /></a></li>
      </ul>
    </section>

and add the following script to your page:

    <script>
       $(document).ready(function() {
         $('.gallery').magnificPopup({
           delegate: 'a', // child items selector, by clicking on it a popup will open
           type: 'image',
           // other options
           gallery:{
           enabled:true
         }
        });
      });
    </script>

That’s it. The lightbox works wonderfully well on all kinds of devices like smartphones, tablets etc …

Finally, to enable full page scrolling, I’m using a library called [fullPage.js](http://alvarotrigo.com/fullPage/) . 
![fullPage.js, a one page scroll library](/images/fullpage-screenshot.jpg)

I have dabbled with a couple of full page scrolling libraries and this is by far the easiest to implement.

To make it work, you need to define each scrollable section with:

       <div class=“section”>
       …
       </div>

Then add the script that enables the full page scroll:

    <script type="text/javascript">
     $(document).ready(function() {
       $('#fullpage').fullpage(
	     {
	     anchors: ['hero', 'activities', 'about', 'team', 'referrals'],
	     autoScrolling: false,
	     fitToSection: false
	     }
       );
    }); 
    </script>

The defined anchors array in this script makes it possible to add anchor links
to the defined sections. When clicked, the page scrolls to the section.

The autoScrolling feature makes the section position itself automatically to
the top when a swipe or click event is triggered. However because not all
content on this website fits on a mobile screen, it was impossible to scroll to
the offscreen content of a section because the page was automatically scrolling to the top, so I had to put
the autoscrolling feauture off.

The fitToSection setting makes the section position itself to the top of the
page each time a scrolling event is triggered. For the same reason as mentioned
above I had to turn this feature off.

The srcset attribute and jQuery libraries enabled me to build a website with a big focus on imagery and an easy image browsing experience.
