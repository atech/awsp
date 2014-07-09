# aTech Web Styling Protocol (AWSP)

This document explains the aTech Web Style Protocol (AWSP). This is a protocol which outlines how a web document should be styled using SCSS & HTML. 

AWSP specifies that no styling information should be present in an HTML document. This means that no HTML element should not have class names which relate to their appearance (for example, 'green', 'large' or 'rightAligned'). In order to layout a document to comply with AWSP, you will need to define a number of CSS documents (known as Views) which can position & style your HTML elements.

The core philosophy is as follows:

* A web document is split into three main concerns: content (HTML), style (CSS) and behaviour (Javascript). There is no place for styles in the content layer. The use of style attributes or classes which make reference to appearance is not permitted.

* Every part of a web document has a direct relationship to a single controlling CSS selector.

* Modular design components are recommended but not required for every element. In some cases, a small design decision may be implemented without constructing a whole modular component to style it.

* While thought should be given to how an element will be used in the future, prematurely optimising classes to support currently un-required functionality is discouraged.

* Deciding where to place additional styles for existing elements (or new elements) should be well defined.

## The structure of an AWSP web document

An AWSP web document will implement a number of objects in the CSS layer.

* **Pages** - whole HTML documents
* **Views** - semantic sections of HTML documents
* **Components** - design elements which can be re-used (for example: buttons, forms, tables etc...)

### What is a page?

A page a whole HTML document. The contents between `<html>` and `</html>` make up the entire contents of a page. A page can set a class on the `<body>` element which sets it's name. This value must be prefixed with `page` followed by a camel case name. For example, `pageHomepage` or `pageFeatures`.

### What is a view?

A view is a section of a page. A view is responsible for positioning & styling all the elements which are contained inside it. A view can be any block-level HTML element (usually a `header`, `footer`, `section` or `div`). The element must have a class which is prefixed with `view` and contains a unique name for the view in camel case. For example, `viewSiteHeader` or `viewFeatureMatrix`.

![Screenshot of Views](http://s.adamcooke.io/14/epB07.png)

In the screenshot above, you can see an example page with each view identified by a blue box. Using the first view as an example, you can see this contains a logo, a set of primary navigation and a couple of buttons (on the right). The markup for such a view may look like this:

```html
<header class='viewSiteHeader'>
  <h1 class='logo'><a href='/'>Sirportly</a></h1>
  <nav class='mainNavigation'>
    <ul>
      <li><a href='/features'>Features</a></li>
      <li><a href='/pricing'>Pricing</a></li>
      <li><a href='/customers'>Customers</a></li>
      <li><a href='/blog'>Blog</a></li>
      <li><a href='/resources'>Resources &amp; Support</a></li>
    </ul>
  </nav>
</header>
```

The styles for this view would be defined as follows in a SCSS document.

```scss
.viewSiteHeader {
  
  .logo {
    // Styles for the logo
  }
  
  .mainNavigation {
    ul {
      li {
        float: left;
        // Other styles for each navigation item.
      }
    }
  }
  
}
```

You will notice that we don't use specify the HTML element name when selecting our elements. We simply use the class selector. This allows the markup to use whatever element is appropriate for the website without worrying about the styles which should be applied. This allows for a fully semantic HTML document to be created.

Also notice that the elements within the view do not include any prefix. Elements which are directly within a view do not need to be prefixed with anything. It is assumed that these names will be unique to the view in question.

### What is a component?

A component is an area of a page which has a defined purpose & style and may need to be re-used somewhere else within the page. A component can be any HTML element. The element must have a class which is prefixed with `com` and contains a unique name for the component in camel case. For example, `comButton` or `comTabBar`.

In the example above, the buttons in the top right utilise components in AWSP. Let's look at the markup for the components. This wasn't included in the markup above but it would be inserted above the main navigation element.

```html
<nav class='comButtonBar metaLinks'>
  <ul>
    <li><a href='/signup' class='signup comButton'>30 day free trial</a></li>
    <li><a href='/login' class='login comButton'>Login</a></li>
  </ul>
</nav>
```

You will see that we have created a `nav` element with the name `comButtonBar` which contains an unordered list with two links with the class `comButton`. This particular example uses two different components. The `comButtonBar` component allows two `comButton` components to be placed into an unordered list and then be displayed next to each other with rounded corners on the left side of the first button and on the right side of the last button. The `comButton` component sets the style of a button - in this case, it's just sets a link to have green background.

Here's the CSS for these component:

```scss
.comButtonBar {
  > ul {
    li {
      float:left;
      // plus styles to add border radius to first/last items
    }
  }
}

.comButton {
  background: green;
  color: white;
  padding: 5px 15px;
}
```

One thing you will notice is that this component does not specify any elements which define its position or margin within the view. The position of a component is the responsibility of the view which contains it. Back in our view file, we can simply position it on the right using the class we assigned to it. In here, we can also set the colour for our login button so that it is grey rather than green (which was the default we set when we originally defined it).

```scss
.viewSiteHeader {
  .metaLinks {
    float: right;
    
    .login {
      background-color: grey;
    }
  }
}
```

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