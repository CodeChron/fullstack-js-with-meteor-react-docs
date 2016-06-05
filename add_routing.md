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

## Create an AppLayout component
In the FlowRouter model, we have a top-level template or componente, in which we define regions where we can insert page-specific content. This is based on a template model, in which we have areas of an app that are global, and in which then define regions where we insert page-specific content.  This model is more suitable for view tools like Blaze, and not really suited for React.  

_talk more here about why this is the case?_

For this reason, we don't want to use this for layout.  Let's instead just create a minimal top-level "App" component.

``` /imports/components/app.jsx ```


```js
import React from 'react'

export const App = (props) => props.page(props)
```

Here we are defining a component whose only job is to return a single region, the "page."  

The way we are doing this is somewhat different from what you might see in the FlowRouter documentation.  Instead of just passing in whatever regions we have defined, we are passing in an object, ```props```, which can contain much more than that, such as reactive data.  Similarly, we are then also passing that object into the region itself, making it available to child components.

## Create a homepage component
Let's create a placeholder component for the homepage.

``` /imports/components/pages/homepage.jsx ```


```js
import React from 'react'

import React from 'react'

export const Homepage = (props) =>
  <div id="app-container">
     {"App Header will go here"}
    <div id="main-content">
      {"Homepage content goes here"}
    </div>
  </div>
```

Now, you might look at this and say that it doesn't look very [DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself).  In  other words, by setting things up in this way, it means we will need to include the ```app-container`` block on every page. 

Yes, that is true, but by doing so we will be able to have a top-level "page" component from which we can manag state for the entire page.

_explain this futher_ - basically, you want to have a single component hierarchy for every page.  React is one-way top-down.  This is part of what allows for keeping React simple and prevents callback soup.  But it also means that if you want to control state for an entire page, your page component needs to be at the very top.

There is a little bit of repetition, but a couple divs is not so bad and I think the trade-off is an easy call.


As we will see, having a top-level "controller" component is a very effective pattern when working with react.

Note that we are passing in the props object from before, though we're not yet making use of it.

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
      page: () => <Homepage />
    })
  }
})
```



In your browser, you should now see "App Header will go here" and "Homepage content goes here."  These are placeholders which we'll soon replace with actual content.

If you trying going to a different URL, you will now see that you just get a blank page and a message from Flow-Router.

![foo-route](https://cloud.githubusercontent.com/assets/819213/15657042/499dc042-267b-11e6-9b77-c9fd0210f2e1.png)



