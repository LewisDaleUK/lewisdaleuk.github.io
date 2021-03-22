---
layout: post
title:  "The case for using HTML test attributes"
date:   2021-03-21 09:00:00 +0000
categories: testing frontend
---
Introduce the problem
* Writing acceptance or integration tests that touch the frontend
* Following TDD: want to write the tests before we do the implementation
* Most testing frameworks (e.g. Selenium) require element selectors
* Show example of what I mean

## Why is this a problem?

* Attributes such as class name are typically implementation details
* They're used to influence how the application functions e.g. styling, semantic elements
* Elements can be moved around to change a page design, without impacting it's actual function
* If we change these elements, we then need to update our tests  

## Why can't we just use ID?
* ID has meaning
* One-per-page
* Still an implementation detail

## The solution: Test attributes

* Encode custom attributes in HTML with `data-*` 
* Can use this to add test IDs to elements

{% highlight html %}
<!doctype html>
<html>
    <head>...</head>
    <body>
        <h1 class="heading" data-test="page-title">Basic Gallery</h1>
        <div>
            <a class=" href="..." data-test="previous-image">Previous image</a>
            <img src="..." data-test="gallery-image" />
            <a href="..." data-test="next-image">Next image</a>
        </div>
    </body>
</html>
{% endhighlight %}

### Pros

* We can restructure the page without worrying about changing our tests
* Can make assumptions about test tag names
    * Enables writing acceptance tests upfront
* No impact on page semantics or visuals

### Cons

* Not a standardised attribute
    * Requires documentation
* Some bloat to page
    * Minor
 
 ---
  
Let's set the scene: You're working on the frontend for an eCommerce project.

To begin with all you need is a simple product listing page, which you've produced, along with some simple integration tests to verify everything is working as it should be; both are shown below.

{% highlight html %}
<!doctype HTML>
<html>
    <head>
        <title>Product Page</title>
        <link rel="stylesheet" type="text/css" href="/public/styles.css" />
    </head>
    <body>
        <main>
            <div class="product" id="product">
                <h1>The Product Title</h1>
                <img src="/public/product-image.jpg" alt="A picture of our product" class="product__image" />
                <p class="product__description">
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Mauris vulputate ex aliquet leo elementum, ac aliquam dolor ultrices. Vivamus odio dui, luctus sed leo eget, porta rhoncus justo.
                </p>
                <button type="button" class="product__add">
                    <span>Add to basket</span>
                </button>
            </div>
        </main>
</html>
{% endhighlight %}