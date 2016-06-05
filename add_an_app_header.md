# Add an App Header

This component should only be responsible for layout.  However, we are also going to add a default page title, since that will be used in most cases when using this component.

## App Header Requirements

Let's review the mockups for the app header.  These are all the various states that we currently are aware of:


![app-header](https://cloud.githubusercontent.com/assets/819213/15806067/d09a60f0-2b08-11e6-9da4-001ee52cf897.png)

From this, we can glean the following requirements:
- Three column layout, vertically and horizonrally centered, with fixed width left/right columns and a center column filling remaining space.
- The center column might contain a page title or a form.
- The left and right columns might contain icons or be empty.

Generally, this means we want to create a component with left, middle, and right side "slots" that can accept any type of component.  The component will be "dumb", ie it does not determine which components are inserted into the slots, but instead leaves that up to a parent component. It's only responsibility is the layout of other components.

In other words, we want to create a Three Column Layout Component

## Create the Three Column Layout Components
Naming a component in this way reminds us of its responsibility. If a component becomes difficult to name, that might be a "smell" that it is responsible for too much.

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
```

- What are props, propTypes, and defaultProps?



## Add the PageTitle Component

``` /imports/components/content/page_title.jsx ```

```js
import React from 'react'

export const PageTitle = (props) => <h1 className="navbar-brand">{props.pageTitle}</h1>

PageTitle.propTypes = {
	pageTitle: React.PropTypes.string.isRequired
}
```


## Add the App Header to Homepage

``` /imports/components/pages/homepage.jsx ```

```js
...
import { AppHeaderLayout } from '../layouts/app_header_layout'
import { PageTitle } from '../content/page_title'

export const Homepage = () => {
	const 
	  appName = "My Notes App",
	  pageTitle = <PageTitle title={appName} />
      
	return  <div id="app-container">
              <AppHeaderLayout headerCenter={pageTitle} />
            <div id="main-content" className="container">
              Main content
            </div>
          </div>
}
```

We are adding the AppHeader only to the homepage because we will want to be able to pass in different data into this and other components depending on the page being viewed.

You should now see the app header appear in the browser.

![App Header added](https://raw.githubusercontent.com/CodeChron/fullstack-js-preview-docs/master/images/add-app-header.png)
