# Making an Animated Slideshow without jQuery

I've seen and used [jQuery](http://jquery.com) to create banner slideshows many, many times. Sometimes it is the only reason why jQuery is even included in a website. In this time where mobile devices are heavily used, we cannot afford to waste precious bytes, and it is important to streamline our code as much as possible. With CSS transitions at our disposal we no longer need to lean against jQuery to make animated slideshows.

## View the Slideshow in Action

[Check out the demo on CodePen](http://codepen.io/starzonmyarmz/pen/EDCwI)

Some of you might be thinking this slideshow won't work in browsers that do not support CSS transitions. This is true, but that doesn't bother me so much. I think the tradeoff is worth it. This slideshow degrades gracefully - if the browser doesn't support CSS transitions it will simply jump from slide to slide without the sliding transition.

## Some Notes About the Code

The HTML can be as complex or as lightweight as you need it to be. The CSS and JS is looking at direct children of the parent element ``.slides``. This means you can use a variety of markup (``img``, ``div``, ``a``, etc…) to suite your needs.

I wrote the CSS using SASS (and Compass) for convenience sake. Since the width and height properties need to be explicitly written in several areas it makes sense to make those variables. If you'd like to see what the compiled CSS looks like, you can view source the example.

The JavaScript is simply swapping out the classes ``.slide-in`` and ``.slide-out`` on the child elements of ``.slides``. I've also set the variable ``int`` which lets you easily change the interval between slide transitions.

## Save Some Bytes

jQuery is awesome, but it isn't always necessary. Now that mobile devices are starting to dominate the market we should be doing whatever we can to save precious bandwidth. If you can cut out jQuery, you'll be saving your user from downloading an almost extra 35k. So the next time you make an animated slideshow, ask yourself _Do I really need to use jQuery here?_


