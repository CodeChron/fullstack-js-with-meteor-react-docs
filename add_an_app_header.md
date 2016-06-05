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

``` /imports/components/layouts/three_column_layout.jsx ```

```js
import React from 'react'

export const ThreeColumnLayout = (props) => {

	return  <div className="flex-row-centered">
	          <div className="flex-left-right-icons">{props.leftCol}</div>
	          <div className="flex-main-content">{props.middleCol}</div>
	          <div className="flex-left-right-icons">{props.rightCol}</div>
	        </div>
}

ThreeColLayout.propTypes = {
  leftCol: React.PropTypes.object,
  middleCol: React.PropTypes.object.isRequired,
  rightCol: React.PropTypes.object
}
```

## Props and PropTypes
Notice that we are passing in argument called ```props```. This is an object passed in from a parent component that contains everything we might want to provide to this component, and any child components.  We'll see more about how this works when we start actually passing in components, functions, and more.

Additionally, we have added a  ```propTypes`` object to our component.  Here, we are defining the type of props that this component will accept.  As you can see it only accepts object props, meaning that we can only pass into it other components.  If we were to attempt to pass in a component of a different type, this would still work, but we would get a warning in our console.

One can think of this as a mini API for a component.  In other words, it allows a developer to look at a component and determine what props can be passed in and how they should be named.


## Naming components
_turn this into a blog post_
In general, I recommend naming a component based on its responsibility. If a component becomes difficult to name, that might be a "smell" that it is responsible for too much and should be divided into multiple components.


## Add the Component to Homepage

``` /imports/components/pages/homepage.jsx ```

```js
...
import React from 'react'
import { ThreeColumnLayout } from '../layouts/three_column_layout'

export const Homepage = (props) =>
  <div id="app-container">
     <ThreeColumnLayout />
    <div id="main-content">
      {"Homepage content goes here"}
    </div>
  </div>
```

If you look in your browser, you'll see that nothing is displayed. This is because we need to pass in content for this component to display.  Let's create a PageTitle component and pass it into the center column..

## Add the PageTitle Component
First, we'll create the component.

``` /imports/components/content/page_title.jsx ```

```js
import React from 'react'

export const PageTitle = (props) => <h1>{props.title}</h1>

PageTitle.propTypes = {
	title: React.PropTypes.string.isRequired
}
```

Here, we see somthing new``isRequired``` _TODO discuss this._

Next, we'll pass this component in as a prop on the homepage.


``` /imports/components/pages/homepage.jsx ```

```js
...
import { PageTitle } from '../content/page_title'

export const Homepage = (props) => {

 const pageTitle = <PageTitle title={"My Notes App"} />

  <div id="app-container">
     <ThreeColumnLayout middleCol={pageTitle} />
    <div id="main-content">
      {"Homepage content goes here"}
    </div>
  </div>
}
```

_Discuss: we are assigning the PageTitle component to a variable and passing in a hard-coded prop.  This is something we would want to refactor.  Any time you see hard-coded text that is a smell.  If this were a real project, I would likely add a todo item to the backlog to define this value in an App library or other general configuration, where we would set the app name globally.

We are adding the AppHeader only to the homepage because we will want to be able to pass in different data into this and other components depending on the page being viewed.

You should now see the app header appear in the browser.

![App Header added](https://raw.githubusercontent.com/CodeChron/fullstack-js-preview-docs/master/images/add-app-header.png)
