# Add Styling

We are going create our styling from scratch and not use, for example, Bootstrap.  While Bootstrap is great, creating a framework on your own is great for learning purposes.  Additionally, for larger apps, if you started out with Bootstrap, you might find yourself "figthing" or constantly over-riding what Bootstrap did with every new customization.

## Remove the default css file
It doesn't do anything, but we don't want "dead" files in our app.

## Add a css preprocessor and auto-prefixer
Modern web apps use so much css that writing it all manually just doesn't make sense.  Instead, let's use some CSS tools.  Here, we'll use Sass and Autoprefixer. (What is [autoprefixing](https://css-tricks.com/autoprefixer/)?)

We need to first remove a package, as we're going to replace it with a community version. Then we'll install the Sass and autoprefixer packages.

``` meteor remove standard-minifier-css ```
```  meteor add fourseven:scss seba:minifiers-autoprefixer ```

## What is our css strategy/approach?


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

This is just a foundation.  We'll add more styling while building the app itself.




