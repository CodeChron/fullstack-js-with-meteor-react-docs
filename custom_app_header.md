# Add Custom Styling



## Update the note header to have a custom 3 column layout

We want our note title to be centered and we'll want a back icon to be left aligned. This will require customizing our app header layout. 


## Add CSS Pre-processing
Modern web apps use so much css that writing it all manually just doesn't make sense.  Instead, let's use some CSS tools.  Here, we'll use Sass and Autoprefixer. (What is [autoprefixing](https://css-tricks.com/autoprefixer/)?)

### Install Sass and Autoprefixer packages

We need to first remove a package, as we're going to replace it with a community version. Then we'll install the Sass and autoprefixer packages.

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

.three-col-layout {
	display: flex;
  flex-flow: row nowrap;
  align-items:center;

  .main-content {
  	flex: 1;
  }
  
  .left-right-icons {
    width: 3em;
  }

}

// APP HEADER
// ____________________________

.navbar {
  height: $app-header-height;
 
  .navbar-header {

    .navbar-brand {
      margin-top: .25em;
      font-size: $font-size-page-title;
    }
  }
}

// BUTTONS
// ____________________________

.icon-btn{
  background-color: transparent;
  border:none;
  
  margin:0;
   padding:.5em;
  text-align: center;

  &:focus {
   outline: 0;
  }
}

// HELPERS
.centered {
  text-align: center;
}
.full-width {
  width: 100%;
}
```

## Importing existing stylesheets
Now that we are using sass, we should import all other stylesheets into the main stylesheets.  This will ensure that everything gets combined and minified into a single css file.

- Remove imports from: ``` /imports/components/loader/loader.jsx ``` and ``` /imports/components/content/editable_content.jsx ```
- Rename these files to ``` /imports/stylesheets/vendor/_loader.scss ``` and  ``` /imports/stylesheets/_helpers.scss ``` (prefixing the name with an underscore means it will only be loaded if explicitly imported.)
- Import them into the main scss file (add this to the end):

```scss
...
// HELPERS
// ____________________________
@import 'helpers';

// VENDOR
// ____________________________
@import 'vendor/loader';

```

## Import styles on startup
Note that this should be the first file to be loaded.

``` /imports/startup/client/index.js ```

```js
import '../../stylesheets/main'
...
```




## Update the app header to have a three column layout

``` /imports/components/layouts/app_header_layout.jsx ```

```js
...

export const AppHeaderLayout = (props) => {

	return <nav className="navbar navbar-default three-col-layout">
      <div className="left-right-icons">
	    {props.headerLeft}
	  </div>
	  <div className="container">
	    <div className="navbar-header">
	      {props.headerCenter}
	    </div>
	 </div>
      <div className="left-right-icons">
	    {props.headerRight}
	  </div>
	</nav>

}

AppHeaderLayout.propTypes = {
  headerLeft: React.PropTypes.object,
  headerCenter: React.PropTypes.object,
  headerRight: React.PropTypes.object
}

AppHeaderLayout.defaultProps = { 
  headerLeft: null,
  headerCenter: null,
  headerRight: null
}
```

Your page title should now appear as centered in the app header
