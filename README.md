# From jQuery to DOM and ES6

jQuery was created to cope with JavaScript implementation differences between browsers. Internet Explorer was the main culprit and lagged behind Mozilla Firefox and then Google Chrome.

In 2017, IE8 to IE11 usage have droppped to a very small part of web traffic and can be [safely ignored](https://www.microsoft.com/en-gb/WindowsForBusiness/End-of-IE-support). This means that we can drop jQuery from our applicaiton and rely on [standard DOM]() and some ES6 sprinkles.

## Selecting DOM elements

Suppose you have the following HTML:

```html
<div id="lead">lorem ipsum</div>
<ul>
  <li class="green">First item</li>
  <li class="red">Second item</li>
  <li class="green">Third item</li>
  <li class="red">Last item</li>
</ul>
<p class="green status">A great status</p>
```

Then in jQuery you would use the `$()` function in combination with some CSS selector to go and fetch the DOM elements.

```js
var lead = $('#lead');
var firstRedItem = $('.red').eq(0);
var greenListItems = $('li.green');  // Adding `li` not to select the `p`.
```

The alternative is now to use three standard DOM methods:

- [`getElementById`](https://developer.mozilla.org/en-US/docs/Web/API/Document/getElementById)
- [`Element.querySelector`](https://developer.mozilla.org/en-US/docs/Web/API/Element/querySelector)
- [`Element.querySelectorAll`](https://developer.mozilla.org/en-US/docs/Web/API/Element/querySelectorAll)

```js
const lead = document.getElementById('lead');  // ⚠️ no #
const firstRedItem = document.querySelector('.red');
const greenListItems = document.querySelectorAll('li.green');
```

You may have noticed that we used `const` over `var`. Moving forward using ES6, we won't use `var` anymore, but `const` or `let`. We'll use `let` when we want to reassign a variable, `const` otherwise:

```js
let name = "George";
name = name + " Abitbol"; // `name` is being re-assigned.
console.log(name);
```

You can read more on [`let` and `const`](https://medium.com/javascript-scene/javascript-es6-var-let-or-const-ba58b8dcde75)

## Inserting a DOM element

A classic usage of JavaScript would be to insert a new piece of content into the DOM. On a blog, an AJAX-powered comment section would insert the comment when it's submitted, without reloading the page.

```html
<div id="comments">
  <p class="comment">This was great!</p>
  <p class="comment">I loved it :)</p>
</div>
<form>
  <textarea id="commentContent"></textarea>
  <button>Post comment</button>
</form>
```

Using jQuery, this is how we would do it:

```js
var commentContent = $('#commentContent').val(); // Skipping AJAX part
$('#comments').append('<p class="comment">' + commentContent + '</p>');
```

Now we'll rely on [Element#insertAdjacentHTML](https://developer.mozilla.org/en-US/docs/Web/API/Element/insertAdjacentHTML) and [ES6 template literals](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Template_literals):

```js
const commentContent = document.getElementById('commentContent').value;
const comments = document.getElementById('comments');
comments.insertAdjacentHTML('beforeend', `<p class="comment">${commentContent}</p>`);
```

## Updating CSS classes

Adding, removing or toggling a CSS class on a DOM element is something quite common in a JavaScript app. With jQuery, we would use:

```js
$(selector).addClass('bold');
$(selector).removeClass('bold');
$(selector).toggleClass('bold');
```

And this would apply the change to **all** elements matching the given `selector`.

Without jQuery, you can use [`classList`](https://developer.mozilla.org/en/docs/Web/API/Element/classList):

```js
// For one element selected with `getElementById` or `querySelector`
element.classList.add('bold');
element.classList.remove('bold');
element.classList.toggle('bold');

// For multiple elements selected with `querySelectorAll`:
elements.forEach((element) => {
  element.classList.add('bold');
  // etc.
});
```

See? We just used [Array.forEach](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)!

## Event listeners

JavaScript lets your bring dynamic behavior to your webpage. The simplest example we can take is having an `alert` pop up when click on a button.

```html
<button id="useless-btn">Click me!</button>
```

With jQuery, we would use the `.on()` method (or one of its shortcuts):

```js
$('#useless-btn').on('click', function(event) {
  alert('Thanks for clicking!');
});
```

The modern DOM gives you [`addEventListener`](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener) to play with:

```js
const button = document.getElementById('useless-btn');
button.addEventListener('click', (event) => {
  alert('Thanks for clicking!');
});
```

To get information on the clicked element, you can use `event.target` inside the event listener callback. But what was that `=>`? Well, it is called an [arrow function](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions). Those are great to preserve `this` and get rid of the `var that = this` trick. Wes Bos did a [great post and video](http://wesbos.com/javascript-arrow-functions/) on the topic.

## AJAX

When you want to do AJAX, the original implementation relies on [XMLHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/Using_XMLHttpRequest), but nobody wants to use that. jQuery solved this problem ten years ago introducing the `$.ajax` method with its two handy shortcuts `$.get` and `$.post`:

```js
// GET request
$.ajax({
  url: "https://swapi.co/api/people/",
  type: "GET"
  success: function(data) {
    console.log(data);
  }
});

// POST request
const data = { name: "a name", email: "an@email.com" };
$.ajax({
  url: url,
  type: "POST",
  data: data,
  success: function(data) {
    console.log(data);
  }
});
```

With modern browsers, we can rely on [fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch) which provides a nicer API than raw `XMLHttpRequest`.

```js
// GET request
fetch("https://swapi.co/api/people/")
  .then(response => response.json())
  .then((data) => {
    console.log(data);
  });

// POST request
const data = { name: "a name", email: "an@email.com" };
fetch(url, {
  method: 'POST',
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify(data)
})
.then(response => response.json())
.then((data) => {
  console.log(data);
});
```

You can notice the `fetch()` second argument is a hash using more straightforward key names. Parts of the HTTP request are outlined: the method (or verb), the headers and the body.

## Resources

Here are some useful resources you might want to read:

- [You don't need jQuery](https://github.com/oneuijs/You-Dont-Need-jQuery)
- [You might not need jQuery](http://youmightnotneedjquery.com/)
- [A year without jQuery](http://blog.wearecolony.com/a-year-without-jquery/)
