# Install Mcrypt PHP Extension on OSX Yosemite
_Originally published on January, 29, 2015_

I recently updated [Statamic](http://statamic.com/) to 1.10.2 and then promptly started to get [Mcrypt](http://en.wikipedia.org/wiki/Mcrypt) errors. After some Googling, I found some complicated fixes. After some noodling around, here's a simple way to fix it with [Homebrew](http://brew.sh/):

First, figure out which version of PHP you're running. You can do this by opening up Terminal, and running the following command:

```bash
> php -v
```

I happen to be using PHP 5.5.21 locally, so I'll use that in my examples. Next, _tap_ the `homebrew/php` cask:

```bash
> brew tap homebrew/php
```

This next step is sort of optional. You can search for which versions of mcrypt is available to make sure it matches your version of PHP:

```bash
> brew search mcrypt
```

Like I said earlier, I'm running PHP 5.5.21. Make sure you install the version that corresponds with your version of PHP.

```bash
> brew install php55-mcrypt
```

That last command took several minutes to install it on my machine. After it is installed, restart the server, and you should be good to go!
