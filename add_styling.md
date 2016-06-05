# Add Styling

_update this to use SMACCS_ structure

We are going create our styling from scratch and not use, for example, Bootstrap.  While Bootstrap is great, creating a framework on your own is great for learning purposes.  Additionally, for larger apps, if you started out with Bootstrap, you might find yourself "figthing" or constantly over-riding what Bootstrap did with every new customization.

## Remove the default css file
It doesn't do anything, but we don't want "dead" files in our app.

## Add mobile meta tags
This helps ensure proper rendering on mobile devices.
In Meteor, there is no need to include the html tags.  Meteor will do that for you.

``` /client/head.html ```

```html
<head>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
</head>
```

## Add a css preprocessor and auto-prefixer
Modern web apps use so much css that writing it all manually just doesn't make sense.  Instead, let's use some CSS tools.  Here, we'll use Sass and Autoprefixer. (What is [autoprefixing](https://css-tricks.com/autoprefixer/)?)

We need to first remove a package, as we're going to replace it with a community version. Then we'll install the Sass and autoprefixer packages.

``` meteor remove standard-minifier-css ```
```  meteor add fourseven:scss seba:minifiers-autoprefixer ```

## What is our css strategy/approach?

For structuring our css, we will use the [SMACCS](https://smacss.com/) model. I encourage you to take a little time to read through the material on that website.  Additionally, there are many other models to look at, such as [BEM](http://getbem.com/introduction/). On top of that, you should be aware that there is lots of [debate](https://css-tricks.com/the-debate-around-do-we-even-need-css-anymore/) if we should even be using css when building with React.


# Create a master stylesheet
We now want to creat a main stylesheet file, into which we'll import any global styles.

Finally, we now need to import our master stylesheet on startup.

## Add Normalize
At this point, still won't see any actual changes to your design.  That's ok, since we're still working on the foundation.  

If you are creating your css framework, the very thing you will want to do is to normalize your css across browsers and devices.  Let's therefore add  that.  (Learn more about Normalize)

Imo, you should always include <a href="https://necolas.github.io/normalize.css/">normalize</a> when building a web app, because, as stated in the project description...
<blockquote><a href="https://github.com/necolas/normalize.css/">Normalize.css</a> makes browsers render all elements more consistently and in line with modern standards. It precisely targets only the styles that need normalizing.</blockquote>
A couple more pointers:
<ul>
 	<li>This should be the first css that loads.</li>
 	<li>Do not link directly to an online stylesheet, but instead copy and paste the current version of <a href="https://necolas.github.io/normalize.css/4.0.0/normalize.css">the raw normalize css</a> into a local file.  You don't want to risk the remote stylesheet not loading, or be updated, without you knowing it, since this could impact your entire design.</li>
</ul>
With this in mind, copy and paste normalize into a local file and import it at the beginning of the file.
```@import "normalize.css";```


```scss
@import "vendor/normalize";
```

(Why 'vendor'?  This is a common way of referring to code that was written by someone else that we are making use of.)

If your web page now updates to be a sans serif font, then you know everything has been hooked up properly.


# Add Global/App Container Styling
Next, let's add styling for our app container, which effectively is our global styling.


## Add a variables files
One of the many great advantages of using a preprocessor, such as Sass, is that we can define variables.
One great use for variables are values that basically are arbitrary.  In our case, the width the app container is just a number I picked.  If we were working with a designer, then they could help determine the specific value.  When using a variables file, we can collect all these in one place.


``` /imports/stylesheets/_variables.scss ```

```scss
// LAYOUT
// ____________________________
$app-container-width: 50em;
```

## Add App Container Styling

``` /imports/stylesheets/_app_container.scss ```

```scss
// Apply a natural box layout model to all elements, but allowing components to change - http://www.paulirish.com/2012/box-sizing-border-box-ftw/
html,body {
  box-sizing: border-box;
}
*, *:before, *:after { box-sizing: inherit; }

html {
  height: 100%;
}

body {
  // min-height is needed for pages that might scroll, ie they may contain _more_ than 100% of viewport height
  min-height: 100%;
}

#app-container {
 //set the app container to fill the viewport height
 height: 100vh;

 //center the app in larger viewports
 max-width: $app-container-width;
 margin: 0 auto;

}
```

## Import the files into the main stylesheet

Don't forget to import the file into the master stylesheet before any other styles that will make use of the variables.

``` /imports/stylesheets/main.scss ```

```scss
@import "variables";
@import "vendor/normalize";
@import "app_container";
```


## Add AppContainer styling to the AppLayout component

``` /imports/components/layout/app_layout.jsx ```

```js
...

export const AppLayout = (props) => <div id="app-container">{props.content(props)}</div>
```

(You might be wondering why we are not naming this component "AppContainer" to be consistent.  This is, in part, due to that we will soon be creating a data container that will wrap around this component, called AppContainer.)

This is just a foundation.  We'll add more styling as we build the app itself.




