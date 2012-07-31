Filtering Options in a Select Box with jQuery
=============================================

I recently created a web form for a client that had two elements, where the options presented to the user in the second element were determined by the option selected in the first element. It is a nifty feature to automatically filter options for the user, while providing validation at the same time. It can be a time saver for the user, even if it is only a matter of milliseconds. It also makes form validation stronger knowing that your user won’t get sloppy when filling out your form.

Take out the Guess Work
-----------------------

In my example, the first element contains primary drink categories, and the second element contains specific drinks that fall under one of those categories. It might be confusing to some users if they were to select “Juice” as their primary option and included in their secondary options were “Sam Adams” and “Coke”. Let’s assume that the user submitted the form, having selected “Juice” and “Sam Adams”. What the heck are you suppose with that data? You don’t know if the user wants some sort of juice or specifically “Sam Adams”. Why give the user an opportunity to make a mistake? Take the guess-work out of the form and make it as simple as possible for the user to interact with.

Using the <a href="http://jquery.com">jQuery</a> library and the JavaScript provided in this tutorial, we will be able to validate these fields, making sure that the user actually selects a drink and flavor. We will also be able to filter flavor results based on what the user chooses for a primary drink. This means if the user selects “Soda” for a drink, we’ll only let them see the flavors: “Coke”, “Root Beer”, and “Sprite”.

Setting up the HTML
-------------------

In the HTML below, I’ve created two elements, each with several options. The first element contains drink options for choosing a drink category. The second element contains flavor of the drink. You’ll notice that in the elements nested within the second element we’ve also specified a class attribute which matches up with its parent category. We’ll be using the value of the options in the drink element to match up with the classes in the flavor element in our JavaScript.

We want the second element to be disabled by default, but rather than specifying this in the HTML, we’ll use JavaScript. This way if the user has Javascript disabled, the form will still be functional.

```html
<form>
  <label for="primary">Drink Category</label>
  <select name="primary" id="primary">
    <option value="">-- Select a Drink --</option>
    <option value="Beer">Beer</option>
    <option value="Juice">Juice</option>
    <option value="Soda">Soda</option>
  </select>

  <label for="secondary">Flavor Selection</label>
  <select name="secondary" id="secondary">
    <option class="" value="">-- Select a Flavor --</option>
    <option class="Juice" value="Apple">Apple</option>
    <option class="Beer" value="Budweiser">Budweiser</option>
    <option class="Soda" value="Coke">Coke</option>
    <option class="Beer" value="Corona">Corona</option>
    <option class="Juice" value="Grape">Grape</option>
    <option class="Juice" value="Leomonade">Leomonade</option>
    <option class="Beer" value="Miller">Miller</option>
    <option class="Juice" value="Orange">Orange</option>
    <option class="Soda" value="Root Beer">Root Beer</option>
    <option class="Beer" value="Sam Adams">Sam Adams</option>
    <option class="Soda" value="Sprite">Sprite</option>
  </select>
</form>
```

Making it Cool with JavaScript
------------------------------

This tutorial uses the jQuery library to make life easier for our validation and filtering needs. I’m a big fan of jQuery and I highly recommend it. I realize that using jQuery can add a significant amount of page load time. While we want to be able to validate the form, we also don’t want to have the user wait for JavaScript to load before they start filling out the form. This is why I prefer placing JavaScript right before the closing tag. The first thing we need to do is to include the jQuery library. You can download and host it yourself, or you can do what I do and let Google host it for you.

All of our JavaScript will be in a self-contained function <code>filterSelectBox()</code>. In our function, the first thing we’ll do is disable the second <code>select</code> element. We’re using JavaScript to do this, rather than HTML, in case the user has JavaScript disabled. If we used HTML and the user had JavaScript disabled, the drink element would never become enabled. We’ll also create an array of all the drink options from the second element. Then we’ll assign it to a variable. We’ll be filtering out options in this array based on the the user chooses from the first select box.

```javascript
(function filterSelectBox() {

  $("#secondary").prop("disabled", true);

  var arr = $.makeArray(document.getElementById("secondary"));

})();
```

Now that we have our array assigned to a variable, we want to create an event handler that executes whenever the user <em>changes</em> the state of the first element (which has an id of “category”). In this event handler, we also want to define a couple more variables. The first variable, `primaryCat` will equal the value of the first element, and the second variable, <code>txt</code> equals an empty string.

```javascript
$("#secondary").change(function() {
    var primaryCat = $("#primary").val(),
        txt = "";
});
```

In the following <em>if statement</em>, we want to check to see if the user has selected an option (technically, an option with a value) or not. If the user has, then it will enable the secondary (drink) <code>select</code> element, otherwise, it will disable it.

```javascript
if (primaryCat !== "") {
  $("#secondary").prop("disabled", false);
} else {
  $("#secondary").prop("disabled", true);
}
```

If the user has selected a valid drink category, we want to go through every option in the array of flavor options we created earlier and only keep ones where their class matches the value of the drink option. After we’ve filtered the drink options, we want to flush out all of the current flavor options and replace them with the new ones. We’re only keeping results where the options in the drink select element whose class is equal to the value of the parent category. After we’ve filtered the options, we’re removing all the current options and appending the string of new options to the drink select element.

```javascript
$.each(arr, function() {
  var subCatClass = $(this).attr("class"),
    subCatValue = $(this).val();
  if (subCatClass === primaryCat) {
    txt += '<option class="' + subCatClass + '" value="' + subCatValue + '">' + subCatValue + '<\/option>';
  }
  $("#secondary").empty().append(txt);
});
```

Wait a minute, is it really worth the hassle?
---------------------------------------------

I’d like to assume that the average user is competent enough to fill out a form correctly, but you and I both know that a lot of users are not. At the very least, you should probably be using some sort of basic form validation, whether it be client-side, server-side or both. It doesn’t take too much more time or effort to make things a littler easier on the user by filtering their options — especially if there are a lot of options to choose from. You’re also saving yourself time and energy by applying form validation because you won’t have to try and sort out faulty submissions.
