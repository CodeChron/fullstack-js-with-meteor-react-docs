# Add React and FlowRouter


## Install React 
Starting with Meteor 1.3, we install React packages using npm.

```meteor npm install react react-dom --save```

- Why are we using npm instead of a Meteor package?
- What is the react-dom package?
- ["What is the --save option for?"](http://stackoverflow.com/questions/19578796/what-is-the-save-option-for-npm-install) 


## Install FlowRouter 

Routing is basically the ability to have multiple pages in your app, and to be able to display different content based on the URL in your browser. There are many ways to add routing.  We'll use one of the more popular packages: FlowRouter.

Routing should be added very early on in your app development. One reason for this is that routing is very fundamental to an app's architecture, there are many aspects of your app that will be impacted by which route you currently are viewing.  Think of routing as part of the core plumbing of your app. You don't to add it as early as possible, even if you initially only have only one page in your app.  If not, you might find yourself having to undo work you've done previously.  In our case, we will have to undo some of the tasks we did in the previous branch.

Even though we currently only will have a single page, we still want to add routing, because we know we'll have multiple pages eventually.

If you are using React and planning to use FlowRouter (which we are doing), you should intall them at the same time.  Otherwise, in order to ensure your React installation is working, you will need to create files that you will then need to remove after installing FlowRouter.

Even though we currently only will have a single page, we still want to add routing, because we know we'll have multiple pages eventually.

``` meteor add kadira:flow-router ```
``` npm i react-mounter --save ```



After installing this we should see message from flow-router that there is no 'root route' or basically no homepage.  Let's therefore add a homepage as our first route.

## Create a homepage component

``` /imports/components/pages/homepage.jsx ```


```js
import React from 'react'

export const Homepage = () => <div>{"Homepage content goes here"}</div>
```

## Create a routes file and a homepage route

Let's use the above component for our root route.

``` /imports/routes.jsx ```

```js
import { FlowRouter } from 'meteor/kadira:flow-router'
import React from 'react'
import { mount } from 'react-mounter'
import { App } from './components/app'
import { Homepage } from './components/pages/homepage'

FlowRouter.route('/', {
  name: 'homepage',
  action() {
    mount(App, {
      content: () => <Homepage />
    })
  }
})
```

## Update the App to render route regions

``` /imports/components/layouts/app_layout.jsx ```

```js
import React from 'react'

export const App = (props) => props.content(props)
```

Here, we are calling the region function that we specified in the routes file.  We are also passing along any props into our components.

## Remove "manual" app rendering

FlowRouter handles this for us.

_delete_ the following files:
```/imports/startup/client/main.html ```
```/imports/startup/client/main.js ```

## Import routes on startup

Also, be sure to remove the old main.js import in this file

``` /imports/startup/client/index.js ```

```js
import '../../routes.jsx'
```

In your browser, you should now see "Homepage content" instead of "React placeholder"

If you trying going to a different URL, you will now see that you just get a blank page and a message from Flow-Router.

![foo-route](https://cloud.githubusercontent.com/assets/819213/15657042/499dc042-267b-11e6-9b77-c9fd0210f2e1.png)



