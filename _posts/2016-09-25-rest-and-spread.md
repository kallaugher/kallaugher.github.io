---
layout: post
title:  "The Rest and Spread Operators in ES6"
date:   2016-09-25 09:00:00
categories: code, javascript
---


The rest and spread operators are new features introduced in ES6. Before we dive into them, I’m going to give a quick introduction to destructuring features in ES6 — this context will make the two operators much more straightforward to understand.

Destructuring
--------

Destructuring is a form of syntactic sugar in ES6 that makes easier and less verbose to extract data from objects and arrays in JavaScript. It uses non-strict [pattern matching](http://dmitrysoshnikov.com/notes/pattern-matching/) for variable binding:

{% highlight javascript %}
const simpleArray = [1, 2, 3, 4];
const [a, b] = simpleArray;

console.log(a); //=> 1
{% endhighlight %}

<code>simpleArray[0]</code> and <code>simpleArray[1]</code> are now bound to the variables <code>a</code> and <code>b</code>, even though the two arrays used do not match exactly. We can therefore choose to only bind variables to the properties that we care about.

Note that in the simple example above, we ignored indexes 2 and 3 of <code>simpleArray</code>. If instead we wanted the last two indexes, we can use elision to skip elements in the array:

{% highlight javascript %}
const simpleArray = [1, 2, 3, 4];
const [,, a, b] = simpleArray;

console.log(a); //=> 3
{% endhighlight %}

Destructuring can be used similarly for an object:

{% highlight javascript %}
const obj = { a: 1, b: 2 };
const {b: x} = obj;

console.log(x); //=> 2
{% endhighlight %}

With these features, we can use similar syntax for extracting data as we do for constructing objects, and extract multiple pieces of data at the same time.

The rest operator
-----------------

The rest operator allows us to extract data and assign it to an array. For example:

{% highlight javascript %}
const simpleArray = [1, 2, 3, 4];
const [a, ...b] = simpleArray;

console.log(b); //=> 2,3,4
{% endhighlight %}

The rest operator can also be used for functions taking an arbitrary number of parameters. For example:

{% highlight javascript %}
function sayHello(greeting, ...names) {
  return names.forEach(name => console.log(`${greeting} ${name}!`));
}

sayHello('Hi', 'Annie', 'John', 'Becca');

//=>Hi Annie!
//=>Hi John!
//=>Hi Becca!
{% endhighlight %}

Note that this can only be used once in each function parameter declaration, and must be the last argument.

(The example above also uses the arrow function, another new feature of ES6.)

The spread operator
-------------------

The spread operator is essentially the opposite of the rest operator. Instead of combining values into a single array, it expands arrays into multiple elements.

This allows you to easily combine arrays:

{% highlight javascript %}
let array = [4, 5, 6];
let array2 = [1, 2, 3, ...array]; //=> [1, 2, 3, 4, 5, 6]
{% endhighlight %}

Or to use an array as arguments to a function:

{% highlight javascript %}
function add(num1, num2) { }
let numbers = [1, 2];
add(...numbers);

//previously you would have to use the <code>apply()</code> method:
add.apply(null, numbers);
{% endhighlight %}

Resources:
----------

- A deeper look at destructuring: [Exploring Javascript](http://exploringjs.com/es6/ch_destructuring.html)
- More on the rest operator:[Learning ES6](https://www.eventbrite.com/engineering/learning-es6-rest-spread-operators/)
- More on rest and spread: [Pony Foo](https://ponyfoo.com/articles/es6-spread-and-butter-in-depth)
- If you want to play around with these new features: [ES6fiddle](http://www.es6fiddle.net/)
