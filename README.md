# aTech Web Styling Protocol (AWSP)

This document explains the aTech Web Style Protocol (AWSP). This is a protocol which outlines how a web document should be styled using SCSS & HTML. 

The core philosophy is as follows:

* A web document is split into three main concerns: content (HTML), style (CSS) and behaviour (Javascript). There is no place for styles in the content layer. The use of style attributes or classes which make reference to appearance is not permitted (therefore classes named 'greenButton', 'rightAligned' or 'largeText' are not allowed).

* Modular design components are recommended but not required for every element. In some cases, a small design decision may be implemented without constructing a whole modular component to style it.

* While thought should be given to how an element will be used in the future, prematurely optimising classes to support currently un-required functionality is discouraged.

* Deciding where to place additional styles for existing elements (or new elements) should be well defined.

* Long and clumsy class names are not desired in our markup.

## The structure of an AWSP web document

An AWSP web document will implement a number of objects in the CSS layer.

* **Pages** - whole HTML documents
* **Views** - semantic sections of HTML documents
* **Components** - design elements which can be re-used (for example: buttons, forms, tables etc...)

### What is a page?

A page a whole HTML document. The contents between `<html>` and `</html>` make up the entire contents of a page. A page can set a class on the `<body>` element which sets it's name. This value must be prefixed with `page` followed by a camel case name. For example, `pageHomepage` or `pageFeatures`.

### What is a view?

A view is a section of a page. A view is responsible for positioning & styling the elements which are contained inside it. A view can be any block-level HTML element (usually a `header`, `footer`, `section` or `div`). The element must have a class which is prefixed with `view` and contains a unique name for the view in camel case. For example, `viewSiteHeader` or `viewFeatureMatrix`.

![Screenshot of Views](http://s.adamcooke.io/14/epB07.png)

In the screenshot above, you can see how a page is made up of a number views. To keep things simple, we're now going to look at creating an example view used for displaying a blog. You'll see below that the view contains two areas which will have content - a main content area on the left and a sidebar on the right.

![Screenshot of a blog](http://s.adamcooke.io/14/dDZ3L.png)

```html
<section class='viewBlog'>
  
  <div class='viewBlog__content'>
    <!--- Content goes here -->
  </div>
  
  <div class='viewBlog__sidebar'>
    <!--- Sidebar goes in here -->
  </div>
  
</section>
```

The stylesheet for this view is responsible for positioning these elements on the page. Here's an example stylesheet for our `Blog` view.

```scss
.viewBlog {
  background: white;
}

.viewBlog__content {
  float: left;
  width: 60%;
}

.viewBlog__sidebar {
  float: right;
  width: 40%;
}
```

You will notice that we don't use specify the HTML element name when selecting our elements. We simply use the class selector. This allows the markup to use whatever element is appropriate for the website without worrying about the styles which should be applied. This allows for a fully semantic HTML document to be created.

You will notice that our child elements (content & sidebar) are prefixed with the name of the view joined with a double underscore. All elements which the view is taking responsibility for must be prefixed with the name of the view. The actual definition should be defined at the root within the same file as the view itself.

### What is a component?

A component is a part of your page which has its own styles (colours, sizes, etc...). A component can be any HTML element and may have any number of child elements. The element must have a class which is prefixed with `com` and contains a unique name for the component in camel case. For example, `comButton` or `comTabBar`.

There are two types of component - a block component or an inline component. A block component will contain a number of child objects whereas an inline component will simply modify the styles of an existing element.

Here are some examples of components which you might create in your web document along with their types.

* **Buttons** - inline - a component style for every button (see: http://s.adamcooke.io/14/v603j.png)
* **Form Elements** - inline - styles for text input fields, fieldsets etc...

* **Lists** - block - a style which formats an un-ordered list (see: http://s.adamcooke.io/14/2h3mb.png)
* **Tables** - block - styles for tables containing data (see: http://s.adamcooke.io/14/eiEHD.png)
* **A testimonial** - block - the styles needed to display a testimonial from a customer (see: http://s.adamcooke.io/14/J5pY8.png)
* **Boxes** - block - a box to display a newsletter signup (see: http://s.adamcooke.io/14/Jx46b.png)
* **A blog post** - block - the styles needed to style a complete blog post (see: http://s.adamcooke.io/14/HlNOZ.png)
* **Navigation bars** - block - styles needed to position links into a navigation bar (see: http://s.adamcooke.io/14/Umll1.png)
* **Page titles** - block - styles needed to display page titles (see: http://s.adamcooke.io/14/zDsXk.png)

The actual blog shown above uses 4 block components - one for the blog post, one for the newsletter signup, one for the signup call to action and one for the list of recent posts. In this example, we'll just define a component for the green newsletter signup box. Here's the HTML which would be inserted into the `viewBlog__sidebar` view.

```html
<div class='comNewsletterSignup'>
  <p class='title'>Receive the latest news and tips & tricks from us directly to your inbox.</p>
  <form action='/subscribe' method='post'>
    <p class='inputFields'>
      <input type='text' name='email' placeholder='Enter your email' class='emailField'>
      <input type='submit' name='submit' value='Go' class='submitButton'>
    </p>
  </form>
</div>
```

The SCSS for this component would be as follows (although much of the actual styling has been omitted to ensure clarity).

```scss
.comNewsletterSignup {
  background: green;

  .title {
    color: white;
    margin-bottom: 15px;
  }
  
  .emailField {
    width: 60%;
    float: left
  }
  
  .submitButton {
    width: 60%;
    float: left;
  }
  
}
```

One thing you will notice is that this component does not specify any elements which define its own position or margin. The position of a component is the responsibility of the view which contains it.

You will notice that we have not used any prefixes on the component's child elements. Block components should never be nested within another block component and therefore we can assume that the class namespace is clean and we can use it as needed.

#### Inline components

An inline component is likely a button or an input box. Using a button as an example, we'll demonstrate how you can add a button to a view.

```html
<div class='viewSignupHero'>
  <h2 class='heading'>Sign up today!</h2>
  <p><a href='/signup' class='comButton'>Click here to sign up for a free 30 day trial</a></p>
</div>
```

You'll see we've added a `comButton` class to our link. This now says that this link must look like a button component. The SCSS for this looks like this and simply styles the `comButton`.

```scss
.comButton {
  background: black;
  color: white;
  padding: 6px 16px;
}
```

#### Modifying components

In some cases, a component may have different variations which can be used within your document. For example, you may have different coloured buttons for different use cases. In this example, we'll create styles for two buttons.

```scss
.comButton-green {
  background-color: green;
}

.comButton-red {
  background-color: red;
}
```

We have created two new root level objects prefixed with the full name of the component followed by a hyphen and then the name of our modifier. These should be in the same file as your `comButton` definition. How do you apply these to objects which are on the page I hear you ask. You _could_ just add the `comButton-green` class to your markup but this would break our "no style information in the content layer" rule, therefore we will set the class using our view. Image this HTML:

```html
<section class='viewSignupHero'>
  <h3 class='heading'>Get started with our amazing help desk software!</h3>
  <p><a href='signup' class='comButton signupButton'>Signup today for our free trial!</a></p>
</section>
```

By default, our signup today button will have a black background as that's what we set when we defined our base `comButton`. However, in order to make it use the green modifier we can override it in our view.

```scss
.viewSignupHero {
  .signupButton {
    @extend .comButton-green;
  }
}
```

This uses the SCSS extend functionality which will now automatically link this element with the `comButton-green` modifier we created. If we ever make changes to our modifier, any element which uses it will be updated automatically.


## Guidelines for directory structure

* `components/*.scss` - contains all components (in sub-directories if needed). Each file should define a single `.comXXX` element.
* `views/*.scss` - contains all views (in sub-directories if needed). Each file should define a single `.viewXXX` element.
* `pages/*.scss` - contains any page styles which are needed. Each file should define a single `.pageXXX` element.
* `base/base.scss` - contains base styles for html/body elements if needed.
* `base/typography.scss` - set the default fonts for the document
* `global/variables.scss` - sets any variables
* `global/mixins.scss` - sets any global mixins

## Guidelines for writing CSS & HTML

* Items may only be selected in CSS using a class name, their element (e.g. li or td) or a CSS pseudo selector (e.g. nth-child).

* Any class names must be entered in camel case. For example, 'baseContainer' or 'myFavouriteLlama'.

* In a CSS document, you must not target any element at the root of the document unless it is prefixed with a AWSP prefix or is the `body` or `html` attribute. An exception to this is when using a reset stylesheet which must reset all elements on your page. Reset documents should provide no formatting for any object.

* You must not make assumptions about the structure of an HTML document (with the exception of the objects listed below). For example, when targeting a heading, always select it using a class (for example `.heading`) rather than targeting `h1...h6`. The only occasions when it is acceptable to assume the structure of a element is:

  * Tables - you may select `td`, `tr`, `thead`, `tbody`, `tfoot` and `th` when working with a known `table` element.
  
  * Ordered and Unordered Lists - you may select a first level `li` element when working with a known `ul` or `ol` element.
  
  * Definition Lists - you may select first level `dt` and `dd` elements when working with a known `dl` element.
  
  When building HTML, you must ensure that you assign appropriate class names to elements to ensure they can be selected by an AWSP-compatible stylesheets.

* Never directly position components using the `comXXX` name within a view. Always add a class to the component and use this to position & and override.