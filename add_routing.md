# Add Routing

Routing is basically the ability to have multiple pages in your app, and to be able to display different content based on the URL in your browser.

Routing should likely be added very early on in your app development. One reason for this is that routing is very fundamental to an app's architecture, there are many aspects of your app that will be impacted by which route you currently are viewing.  Think of routing as part of the core plumbing of your app. You don't to add it as early as possible, even if you initially only have only one page in your app.  If not, you might find yourself having to undo work you've done previously.  In our case, we will have to undo some of the tasks we did in the previous branch.

Even though we currently only will have a single page, we still want to add routing, because we know we'll have multiple pages eventually.


## Install Flow-router

There are many ways to add routing.  We'll use one of the more popular packages: FlowRouter.

``` meteor add kadira:flow-router ```
``` npm i react-mounter --save ```

After installing this we should see message from flow-router that there is no 'root route' or basically no homepage.  Let's therefore add a homepage as our first route.

## Create a homepage component

``` /imports/pages/homepage.jsx ```


```js
import React from 'react'

export const Homepage = () => 

<div id="app-container">
  <div id="main-content" className="container">
    Homepage content
  </div>
</div>
```

## Create a routes file and a homepage route

Let's use the above component for our root route.

``` /imports/routes.jsx ```

```js
import { FlowRouter } from 'meteor/kadira:flow-router'
import React from 'react'
import { mount } from 'react-mounter'

import { AppLayout } from '../../components/layouts/app_layout'
import { AppHeaderLayout } from '../../components/layouts/app_header_layout'
import NotesContainer from '../../components/containers/notes_container'

FlowRouter.route('/', {
  name: 'homepage',
  action() {
    mount(AppLayout, {
      header: () => <AppHeaderLayout />,
      content: () => <NotesContainer />
    })
  }
})
```

## Update the App to render route regions

``` /imports/components/layouts/app_layout.jsx ```

```js
import React from 'react'

export const AppLayout = ({header, content}) =>
  <div id="app-container">
    {header()}
    <div id="main-content" className="container">
      {content()}
    </div>
  </div>
```


## Remove "manual" app rendering

FlowRouter handles this for us.

_delete_ the following files:
```/imports/startup/client/main.html ```
```/imports/startup/client/main.js ```

## Import routes on startup

Also, be sure to remove the old main.js import

``` /imports/startup/client/index.js ```

In your browser, you should now see "Homepage content" instead of "React placeholder"

If you trying going to a different URL, you will now see that you just get a blank page.

![foo-route](https://cloud.githubusercontent.com/assets/819213/15657042/499dc042-267b-11e6-9b77-c9fd0210f2e1.png)



