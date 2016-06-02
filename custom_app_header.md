# Custom App Header

Next, we want to add a back button to our header.  But to do that, we need to first customize the layout a bit, to center the title and make room for icons on each side.

## Update the note header to have a custom 3 column layout

We want our note title to be centered and we'll want a back icon to be left aligned. This will require customizing our app header layout. 


## Use Sass and Autoprefixer
This is not required for what we're doing but it will make writing styles less painful in the long run,

### Install packages

We need to first remove a package, as we're going to replace it with a community version.

``` meteor remove standard-minifier-css ```
```  meteor add fourseven:scss seba:minifiers-autoprefixer ```


## Add Custom Styling

``` /imports/stylesheets/main.scss ```

```scss
// VARIABLES
// ____________________________
$app-header-height: 5em;
$font-size-page-title: 1.5em;


// LAYOUTS
// ____________________________
// Three column vertically and horizontally centered layout using Flexbox

.three-col-layout {
	display: flex;
  flex-flow: row nowrap;
  align-items:center;

  .main-content {
  	flex: 1;
  }

}

// APP HEADER
// ____________________________

#app-header {
	height: $app-header-height;

	.page-title {
		text-align: center;
		font-size: $font-size-page-title;
	}
}


// BUTTONS
// ____________________________

.icon-btn{
  background-color: transparent;
  border:none;
  
  margin:0;
  padding: 0;
  text-align: center;

  &:focus {
   outline: 0;
  }
}
```

## Convert loader styles to a scss partial

### Rename ``` /imports/stylesheets/vendor/loader.css ``` to ``` /imports/stylesheets/vendor/_loader.scss ```

### Import the partial

``` /imports/stylesheets/main.scss ```

```scss
// VENDOR
// ____________________________
@import "vendor/loader";
...

```

## Remove the stylesheet import specific to the loader component

``` /imports/components/loader/loader.jsx ```

```js
import React from 'react'

export const Loader = () => <div className="loader">Loading...</div>
```


## Import styles on startup

``` /imports/startup/client/index.js ```

```js
import '../../stylesheets/main'
...
```


## Apply three-column styling to app header

``` /imports/components/layouts/app_header_layout.jsx ```

```js
import React from 'react'

export const AppHeaderLayout = (props) =>
<nav className="navbar navbar-default three-col-layout">
 ...
```


Your page title should now appear as centered in the app header
