# Install React

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