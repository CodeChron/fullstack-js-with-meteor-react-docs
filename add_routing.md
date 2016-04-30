# Add Routing

Routing is basically the ability to have multiple pages in your app, and to be able to display different content based on the URL in your browser.

Routing should likely be added very early on in your app development. One reason for this is that routing is very fundamental to an app's architecture, there are many aspects of your app that will be impacted by which route you currently are viewing.

## Install Flow-router
After installing this we should see message from flow-router that there is no 'root route' or basically no homepage.  Let's therefore add a homepage as our first route.

There are many ways to add routing.  We'll use one of the more popular packages: FlowRouter.

``` meteor add kadira:flow-router ```
``` npm i react-mounter --save ```


## Add a homepage
Our homepage will display basically the same content as we currently see in the app, but it will be explicitly defined as being on the homepage and accessible via a specific URL ("/")



## Add a base layout
I prefer to just think of this as the app container. Everything we place here will be global to the entire app.
If you have multiple layouts, you can choose to name this "main_layout' or the like.

```
import React from 'react'

export const App = ({content}) => <div>{content()}<Alert /></div>
```
Discuss the {content} prop.


## Add a homepage (root) route

``` /client/routes.jsx ```

```js
import React from 'react'
import {mount} from 'react-mounter'

import {App} from './app'
import {AppHeader} from './layouts/app_header'
import {NotesList} from './containers/notes_list'


FlowRouter.route('/', {
  name: 'homepage',
  action() {
    mount(App, {
      header: () =>  <AppHeader />,
      content: () => <NotesList />
    })
  }
})

```
