# My First Sketch Plugin and What I learned
_Originally published May 16, 2016_

I've been Adobe Illustrator free for almost six months now, and what I miss most about it are some of the nice effects it ships with. I love Sketch, but I don't think these effects will ever make their way into Sketch. Sketch does have the ability to add plugins, and I've always wanted to try my hand at creating one. I really miss is Illustrator's _Roughen_ effect as it's really useful for creating that trendy-old-timey-beat-up look. Since a Sketch plugin that _roughens_ a path doesn't already exist (that I could find) I decided to create one!

I'm going to be up front, and this is mostly a decompression of all the frustration I felt while working on the plugin.

### Delete and reinstall * Infinity

I spent a lot of time just uninstalling, reinstalling the plugin any time I made a change to the plugin script. This was super annoying, but I'm not entirely sure if there was a way around this. Using the **Custom Plugin** menu option was quick and useful for testing out snippets of code, but it's not a fully fledged code editor, so I wouldn't want to use it for sole development. Using `print()` was sometimes useful for logging data to the console, but a lot of the times what was being logged was utter nonsense and useless. This brings me to my next topic.

### Documentation Wasn't Too Useful

I admit I'm not a hardcore developer, but I like to think I'm seasoned enough that I can review some documentation and forge ahead. Looking at the docs on Apple's site was like reading a Unix software manual out of the 80's. It didn't help me out so much. Not only that, Googling Appkit or Cocoa (to be honest I don't even understand which is which) methods didn't help either. Is there a secret club you have to belong to to get useful examples and tutorials?

I'm good with JavaScript, so CocoaScript was nice for bridging the gap. However it's really weird to me that you can write code in Cocoa notation in CocoaScript. Pick a syntax variation and stick with it!

The Sketch API docs are incomplete, and the examples that were there weren't entirely useful. They must also be restructuring because halfway through development all the docs and links changed. Some content was removed entirely that I found kind of useful. Thank God for Google caching! I learned the most from looking at other plugins that did similar things I was looking to do. I also found [this resource](https://github.com/turbobabr/Sketch-Plugins-Cookbook) invaluable - there is so much great stuff in there.

### Late to the Party

So while I'm decompressing from this project I'm remembering I belong to the [Team Sketch](http://teamsketch.io/) Slack Team. I wish I had remembered this earlier because there's a channel dedicated to plug in development, and I'm sure I could've have received a lot of help from there. Just scrolling through the last 50 or so messages I saw an assortment of links that would have been useful.

### Wrap Up

I know I complained a lot in this post. Despite the things that I felt held me back, I feel like I can improve my plugin quite a bit, so I wouldn't say I'm done. I'd like to utilize the Team Sketch Slack Team and some of the resources I found.
