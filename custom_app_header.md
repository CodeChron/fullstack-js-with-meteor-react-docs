# Custom App Header

## Update the note header to have a custom 3 column layout

We want our note title to be centered and we'll want a back icon to be left aligned. This will require customizing our app header layout. 


## Use Sass and Autoprefixer
This is not required for what we're doing but it will make writing styles less painful in the long run,

### Install packages

We need to first remove this package, as we're going to replace it with a community package:

``` meteor remove standard-minifier-css ```
```  meteor add fourseven:scss seba:minifiers-autoprefixer ```



