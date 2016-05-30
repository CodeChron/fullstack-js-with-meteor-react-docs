# Add an App Header Layout Component

This component should only be responsible for layout.  However, we are also going to add a default page title, since that will be used in most cases when using this component.

``` /imports/components/layouts/app_header_layout.jsx ```

```js
import React from 'react'
import { PageTitle } from '../content/page_title'


export const AppHeaderLayout = (props) => {

	return <nav className="navbar navbar-default">
	  <div className="container">
	    <div className="navbar-header">
	      {props.headerCenter}
	    </div>
	 </div>
	</nav>

}

AppHeaderLayout.propTypes = {
  headerCenter: React.PropTypes.object
}

AppHeaderLayout.defaultProps = { 
  headerCenter: <PageTitle />
}
```

- What are props, propTypes, and defaultProps?

## Add the PageTitle Component

``` /imports/components/content/page_title.jsx ```

```js
import React from 'react'

export const PageTitle = (props) => <h1 className="navbar-brand">{props.pageTitle}</h1>

PageTitle.propTypes = {
	pageTitle: React.PropTypes.string
}

PageTitle.defaultProps = { 
  pageTitle: "My Notes App"
}
```

### Avoiding hard-coded strings
We'd likely want to move the "My Notes App" into some form of app info library,but this is ok for now.


## Add  the App Header to the App Layout

``` /imports/components/layouts/app_layout.jsx ```

```js
...
import { AppHeaderLayout } from './app_header_layout'

export const AppLayout = () =>
  <div id="app-container">
    <AppHeaderLayout />
    <div id="main-content" className="container">
      Main content
    </div>
  </div>
```

You should now see the app header appear in the browser.

![App Header added](https://raw.githubusercontent.com/CodeChron/fullstack-js-preview-docs/master/images/add-app-header.png)

Try adding a custom ``` pageTitle ```, eg

``` <AppHeaderLayout pageTitle="My Custom Title" /> ``` 

and you'll see the page title update.