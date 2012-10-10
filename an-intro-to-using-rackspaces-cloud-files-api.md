An Intro to Using Rackspace’s Cloud Files API
=============================================

<em>Originally published on January 24, 2011</em>

I recently created a custom PHP media gallery for a friend of mine. The catch is that all of the media is being served from a Rackspace Cloud File server, while the code is being executed/served from a different server. The reason for doing this is to prevent bandwidth limiting issues from the host, and to cut down on latency issues. I looked and looked for the perfect out-of-the-box PHP Media Gallery script, but came up dry. None of them were capable of serving media from a different server. I had to put my designer’s cap aside, and pull out my dusty, back-end developer’s cap.

This article doesn’t explain how I built the media gallery, but rather it gets into the code I used to take advantage of Rackspace’s Cloud Files API. When I was researching how to use the API, I found very little helpful intros, tutorials, or examples that made sense to… how do I say it nicely… a not-so-savvy, back-end developer like me. My hope in sharing this article is twofold:

1. It will serve as an entry-level introduction to using the Cloud Files API, and
2. You will see some useful functions provided by the Cloud Files API

Do You Have Any References?
---------------------------

The first half of this code I picked up from [Rackspace’s Cloud Files Wiki](http://cloudfiles.rackspacecloud.com/index.php/Sample_PHP_Application). I was disappointed to not find it better documented other than the few inline comments, but I suppose there isn’t really much to document. The rest I pieced together using the [cloudfiles.php](http://github.com/rackspace/php-cloudfiles/blob/master/cloudfiles.php) include file. It’s probably worth going through the [Cloud Files Development Guide](http://docs.rackspacecloud.com/files/api/cf-devguide-latest.pdf) so you can get an idea of what functions are available with the Cloud File’s API.

Before I start, here’s a list of requirements listed in the PHP bindings [README](http://github.com/rackspace/php-cloudfiles/blob/master/README) file:

- PHP version 5.x
- PHP’s cURL module
- PHP enabled with mbstring (multi-byte string) support

I think most reputable hosts at this point have curl installed and mbstring enabled. If you’re not sure (like I wasn’t) create a file with the [phpinfo()](http://www.php.net/manual/en/function.phpinfo.php) function, access the file in the browser and use your browser’s “find” command to look for “curl” and “mbstring”. The README file also recommends you have the PEAR FileInfo module, but it isn’t necessary unless you’re doing Content-Type detection.

Let’s Look at Some Code Shall We
--------------------------------

I’ll start by showing the final code that I came up with for accessing the Cloud Files server, access containers on the server, and retrieve objects’ (images and videos in my case) URIs. All of this is achieved using the Cloud Files API.

```php
<?php

// Includes the Cloud Files PHP API
require('cloudfiles.php');

// Create a new instance of the authentication Class
$auth = new CF_Authentication(<USERNAME>, <API KEY>);

// Returns a valid storage token and allows you
// to connect to the Cloud Files Platform
$auth->authenticate();

// Allows us to connect to Cloud Files and
// make changes to containers
$conn = new CF_Connection($auth);

// Get the container where the client's assets are stored
$cont = $conn->get_container(<CONTAINER NAME>);

// Return Array of the objects inside the client's container
$objs = $cont->list_objects();

// Get the URI for the client's container
$cdnuri = $cont->cdn_uri . "/";

// Go through the Array of objects and output them with links
for ($i = 0; $i < (count($objs)); $i++) {
  // Only show objects that don't have
  // a period in the object name
  if (strpos($objs[$i], ".") !== false) {
    echo '<a href="' . $cdnuri . $objs[$i] . '">' . $cdnuri . $objs[$i] . '</a>';
  }
}

?>
```

Now I’ll take the time to go through each line of the code to further explain it.

```php
require('cloudfiles.php');
```

I probably don’t need to further explain this, but this required file contains the Cloud Files API PHP bindings. You obviously can’t run any of this code without it, and if you try to, you’ll get errors. Also, you don’t need to include the following files, but they need to be in the same location as [cloudfiles.php](http://github.com/rackspace/php-cloudfiles/blob/master/cloudfiles.php): [cloudfiles_exceptions.php](http://github.com/rackspace/php-cloudfiles/blob/master/cloudfiles_exceptions.php) and [cloudfiles_http.php](http://github.com/rackspace/php-cloudfiles/blob/master/cloudfiles_http.php).

```php
$auth = new CF_Authentication(<USER NAME>, <API KEY>);
$auth->authenticate();
```

Creates a new instance of the authentication Class and returns a valid storage token (assuming you use the correct username and API key), thus allowing you to connect to the Cloud Files platform. Use your Rackspace Cloud username as the username for the API, and obtain your API access key from the Rackspace Cloud Control Panel in the Your Account > API Access section.

```php
$conn = new CF_Connection($auth);
```

This command is what actually allows us to connect to Cloud Files and make changes like creating and deleting containers. It also allows us to return existing containers, and their properties.

```php
$cont = $conn->get_container(<CONTAINER NAME>);
```

As it indicates, this gets a specific container. You’ll want to replace <CONTAINER NAME> with the actual name of the container. In my client’s case, there is a separate container for each client.

```php
$objs = $cont->list_objects();
```

The objects in a container are the media assets: image and video files. The list_objects() function returns the objects’ URI in an array. There are some parameters you can pass into this function, which you can use to limit the number of items returned, as well as filter the results. In my case, using the function without any parameters worked just find, but take a look in the required [cloudfiles.php](http://github.com/rackspace/php-cloudfiles/blob/master/cloudfiles.php) file to see the list of the parameters you can pass in.

There’s an important thing to note with objects inside of containers. You can create directories inside of containers, but they’re not real directories. These pseudo directories are treated as objects, just like normal files. Because they’re treated like objects, they’ll also be returned using the list_objects() function. Understanding this will shed some light on the for statement that goes through the array, which I’ll describe more in a little bit.

It’s also important to know that the list_objects() function does not include the URI, which brings us to the next line of code.

```php
$cdnuri = $cont->cdn_uri . "/";
```

I had originally hardcoded the URI of the container I was working with while coding this up, but quickly started running into issues when testing the code with different containers. Silly me for not realizing this earlier, but each container has a unique URI. After I learned that, I realized I needed a dynamic way to determine the containers URI. In my coding, I accidentally discovered that calling the get_container() function returns an array with all kinds of cool information related to the container, including the containers full URI. We add a backslash to the end of this so when we combine it with the objects URI, we have a valid URL.

```php
for ($i = 0; $i < (count($objs)); $i++) {
  if (strpos($objs[$i], ".") !== false) {
    echo '<a href="' . $cdnuri . $objs[$i] . '">' . $cdnuri . $objs[$i] . '</a>';
  }
}
```

This for statement goes through every object in the array that was returned using the list_objects() function so that we can output a link to each object (image or video file in my case). I mentioned earlier that the list_objects() function also returns pseudo directories. If you try to view one of these pseudo directories, you’ll get a 404 error page. Knowing this, it doesn’t make much sense to output these pseudo directories.

The if statement checks to see if the current object in the for statement contains a “.” or a period. If it does contain a period, than it outputs it, otherwise it does not. All the hardcore developers that see this will take their computer and throw it out the window in a fit of rage. I realize this probably isn’t the best solution to my problem, but it’s theory is simple: I’m assuming an object containing a period does so because it has a file extension in the file name (like .jpg or .mp4). This means it’s a real object that can be linked to. In order for this code to work, the client understands that he can’t use periods in pseudo directory names – or they will be output with the real objects.

Don’t Be Shy, Dive Right In
---------------------------

I highly recommend going through the [cloudfiles.php](http://github.com/rackspace/php-cloudfiles/blob/master/cloudfiles.php) file. There’s some good documentation in there that will shed some extra light on some the code I’ve featured here. In fact, as far as documentation goes, I haven’t found anything better. Hopefully this guide will show you that it is pretty easy to use the Cloud Files API. There is so much more that I didn’t talk about like the ability to create and remove containers, objects, as well as accessing containers’ and objects’ metadata.
