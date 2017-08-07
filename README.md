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
const lead = document.getElementById('lead');  // ⚠️   no #
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

## TODO

Coming very soon:

- [ ] [Arrow functions](http://wesbos.com/javascript-arrow-functions/)
- [ ] Event listeners
- [ ] AJAX

## Resources

Here are some useful resources you might want to read:

- [You Might Not Need jQuery](http://youmightnotneedjquery.com/)
- [A year without jQuery](http://blog.wearecolony.com/a-year-without-jquery/)


