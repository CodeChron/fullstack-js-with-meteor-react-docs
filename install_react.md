# Add React

Starting with Meteor 1.3, we install React packages using npm.

```meteor npm install react react-dom --save```

- Why are we using npm instead of a Meteor package?
- What is the react-dom package?
- ["What is the --save option for?"](http://stackoverflow.com/questions/19578796/what-is-the-save-option-for-npm-install) 


## Replace Blaze with a React render target

Replace everything in the file ``` /imports/startup/client/main.html ``` with the following:

```html
<body>
  <div id="app"></div>
</body>
```

This will be our "render target" for React components.


## Add a top-level React component

``` /imports/components/app ```
```js 
import React from 'react'

export const App = () =>
  <div id="app-container">
    <div id="main-content" className="container">
      React placeholder
    </div>
  </div>
```

This is the same as if we had written:

```js
export const App = () => {
 return React.createElement('div', {id: "app-container"}, 
    React.createElement('div', {id: "main-content", className: "container" }, "React placeholder")
 	)
}
```

- What is the '=>' thing?
- Isn't this just a plain JS function? Why are we not using React.createClass or React.Component?
- Why are we using 'className' rather than class?


## Tell Meteor to render our top-level component on startup:

Replace everything in the file ``` /imports/startup/client/main.js ``` with the following:

```js
import React from 'react'
import ReactDOM from 'react-dom'
import { App } from '/imports/components/app'

import './main.html'

Meteor.startup(() =>
	ReactDOM.render(<App />, document.getElementById("app"))
)
```

You should now see this in your browser:

![Dflt view with React added](https://raw.githubusercontent.com/CodeChron/fullstack-js-preview-docs/master/images/react-added-dflt.png)
