# Add React

Starting with Meteor 1.3, we install React packages using npm.

```meteor npm install react react-dom --save```

- Why are we using npm instead of a Meteor package?
- What is the react-dom package?
- ["What is the --save option for?"](http://stackoverflow.com/questions/19578796/what-is-the-save-option-for-npm-install) 



## Add an app layout component

``` /imports/components/layouts/app_layout.jsx ```
```js 
import React from 'react'

export const AppLayout = () =>
  <div id="app-container">
    <div id="main-content" className="container">
      React placeholder
    </div>
  </div>
```

- What is the '=>' thing?
- Isn't this just a plain JS function? Why are we not using React.createClass or React.Component?
- Why are we using 'className' rather than class?


## Replace Blaze with a React render target

Add a location where we want to render our React components:

``` /imports/startup/client/main.html ```

```html
<body>
  <div id="app"></div>
</body>
```

Tell Meteor to render our top-level ```AppLayout``` component at this location on startup:

``` /imports/startup/client/main.js ```

```js
import React from 'react'
import ReactDOM from 'react-dom'
import { AppLayout } from '/imports/components/layouts/app_layout'

import './main.html'

Meteor.startup(() =>
	ReactDOM.render(<AppLayout />, document.getElementById("app"))
)
```

You should now see this in your browser:

![Dflt view with React added](https://raw.githubusercontent.com/CodeChron/fullstack-js-preview-docs/master/images/react-added-dflt.png)
