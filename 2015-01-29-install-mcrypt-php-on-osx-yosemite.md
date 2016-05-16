---
layout: post
title:  Install Mcrypt PHP Extension on OSX Yosemite
date:   2015-01-29
---

I recently updated [Statamic](http://statamic.com/) to 1.10.2 and then promptly started to get [Mcrypt](http://en.wikipedia.org/wiki/Mcrypt) errors. After some Googling, I found some complicated fixes. After some noodling around, here's a simple way to fix it with [Homebrew](http://brew.sh/):

First, figure out which version of PHP you're running. You can do this by opening up Terminal, and running the following command:

{% highlight bash %}
> php -v
{% endhighlight %}

I happen to be using PHP 5.5.21 locally, so I'll use that in my examples. Next, _tap_ the `homebrew/php` cask:

{% highlight bash %}
> brew tap homebrew/php
{% endhighlight %}

This next step is sort of optional. You can search for which versions of mcrypt is available to make sure it matches your version of PHP:

{% highlight bash %}
> brew search mcrypt
{% endhighlight %}

Like I said earlier, I'm running PHP 5.5.21. Make sure you install the version that corresponds with your version of PHP.

{% highlight bash %}
> brew install php55-mcrypt
{% endhighlight %}

That last command took several minutes to install it on my machine. After it is installed, restart the server, and you should be good to go!
