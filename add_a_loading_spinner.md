# Add a loading spinner

Let's display an animation while we wait for our subscription to be ready. This may not seem important when running locally, but when running on a slow network connection, it can be good to provide some feedback.
There are a ton of options for doing this.  

## Create a loading feedback component

``` /imports/components/utlity/loading_feedback.jsx ```

```js
import React from 'react'

export const LoadingFeedback = () => <div className="loader">Loading...</div>
```

## Pass down a prop we can use to tell if subscriptions are ready

First, we need to provide the list component with a way of knowing if subcriptions are ready.  We'll pass this info down from the container.

``` /imports/components/containers/homepage_container.js ```

```js
...

export default createContainer(
	() => {
  ...

	  return {
	  	subsReady: sub.ready(),
	   ...
	  }
 ...
)
```

Next, add the Loading feedback component to the Homepage and add a handler for displaying it if subs are not ready.


``` /imports/components/pages/homepage.jsx ```

```js
...
import { LoadingFeedback } from '../utility/loading_feedback'

export const Homepage = (props) => {

	const
     ...
     ,
	  displayList = () => props.subsReady? <List collection={props.notes} /> : <LoadingFeedback />


  return <div id="app-container" className="l-app-full-height l-app-centered">
         ...
             <SingleFieldSubmit
               handleSubmit={props.handleCreateNote}
               placeholder={"New Note..."}
             />
            {displayList()}
           </div>
     ...
```

Now, in your browser, you should see "Loading" display briefly before the list loads.


## Add a loading animation styling

There are many ways you can add a loading animation.  Here, we'll go to   http://projects.lukehaas.me/css-loaders/ and pick a loader.  Click on "View Source" and copy the css.

You can also just grab this css:

```css
/*
Src: http://projects.lukehaas.me/css-loaders/
*/

.loader,
.loader:before,
.loader:after {
  background: #f0eff1;
  -webkit-animation: load1 1s infinite ease-in-out;
  animation: load1 1s infinite ease-in-out;
  width: 1em;
  height: 4em;
}
.loader:before,
.loader:after {
  position: absolute;
  top: 0;
  content: '';
}
.loader:before {
  left: -1.5em;
  -webkit-animation-delay: -0.32s;
  animation-delay: -0.32s;
}
.loader {
  color: #f0eff1;
  text-indent: -9999em;
  margin: 88px auto;
  position: relative;
  font-size: 11px;
  -webkit-transform: translateZ(0);
  -ms-transform: translateZ(0);
  transform: translateZ(0);
  -webkit-animation-delay: -0.16s;
  animation-delay: -0.16s;
}
.loader:after {
  left: 1.5em;
}
@-webkit-keyframes load1 {
  0%,
  80%,
  100% {
    box-shadow: 0 0;
    height: 4em;
  }
  40% {
    box-shadow: 0 -2em;
    height: 5em;
  }
}
@keyframes load1 {
  0%,
  80%,
  100% {
    box-shadow: 0 0;
    height: 4em;
  }
  40% {
    box-shadow: 0 -2em;
    height: 5em;
  }
}
```

Paste the css into the following file:
``` /imports/stylesheets/_loader.scss ```

Then import the css into your master stylesheet file:

``` /imports/stylesheets/mains.scss ```

```scss
...

//VENDOR 
// 3rd party styling 
// _____________________________________________________________
@import "loader";

```

You should now see an animation appear while your list data loads.


