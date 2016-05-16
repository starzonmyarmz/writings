---
layout:    post
title:     Clarifying CSS with Linking Verbs
date:      2015-02-18
crosspost: http://techtime.getharvest.com/blog/clarifying-css-with-linking-verbs
---

While cleaning up code throughout Harvest, I've been addressing how we indicate how an element’s *state* has changed. Some classic examples you might be familiar with include when an element is *hidden*, *visible*, or *active*. I'll use the *hidden* example to show you how I prefer to indicate an element's state.

In Harvest, we have this reusable utility:

{% highlight css %}
.is-hidden {
  display: none !important;
}
{% endhighlight %}

When we apply this class to an element, that element is hidden. There are other class names I could have used, such as `.hide` or `.hidden`, but there are some general guidelines I like to follow when naming a class to indicate state change.

### State Modifiers Should Start with a Linking Verb

[Linking verbs](http://examples.yourdictionary.com/examples-of-linking-verbs.html) connect the subject of the verb to additional information about the subject. In short, they make text easier to read and provide greater clarity. When you're talking to someone, do you say, "That button hidden?" No! You say, "That button *is* hidden." The most common helping verbs I use when indicating state are *is* and *has*. The state of the `<button>` in this example is easier to read when looking at the class names:

{% highlight html %}
<button class="button is-hidden">…</button>
{% endhighlight %}

### State Modifiers Shouldn't Be Unnecessarily Verbose

There isn’t really a problem with a class name like `.button-is-hidden`, but it is a little unnecessarily *wordy*.

{% highlight html %}
<button class="button button-is-hidden">…</button>
{% endhighlight %}

A reusable class `.is-hidden` that could be applied to buttons as well as to other elements results in less code and is easier to maintain. Sometimes exceptions need to be made, though! Let's switch to a different example for this next scenario. If your `.button` class needed some additional styles, we would modify the state class when chained with the `.button` class, like so:

{% highlight css %}
.has-loading-animation {
  color: red;
}

.button.has-loading-animation {
  background: blue;
}
{% endhighlight %}

This allows us to have one reusable state modifier, while also having a variation of it to use with buttons. Plus, this keeps the code easy to maintain.

### The Real Shebang

Our expense entry form at Harvest uses a custom component for attaching receipt images. This is a very watered down example, but allows you to see how we use the state modifier to toggle the different states.

<iframe width="100%" src="//jsfiddle.net/pch8xvuy/1/embedded/result,html,css,js/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

When there's no receipt, we show the `.attach-receipt` element and its children, and hide the `.remove-receipt` element by adding the state modifier class `.is-hidden`. When the receipt is attached, we remove the class `.is-hidden` from the `.remove-receipt` element, and add `.is-hidden` to the `.attach-receipt` element.

That's it! That's how we do state modifiers in CSS at Harvest. It's simple, easy to read, and easy to understand.
