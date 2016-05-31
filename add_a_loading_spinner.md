# Add a loading spinner

Let's display an animation while we wait for our subscription to be ready. This may not seem important when running locally, but when running on a slow network connection, it can be good to provide some feedback.
There are a ton of options for doing this.  

## Create a loader component

``` /imports/components/loader/loader.jsx ```

```js
import React from 'react'

export const Loader = () => <div className="loader">Loading...</div>
```


### Get the css
Go to http://projects.lukehaas.me/css-loaders/ and pick a loader of your choice.  Click on "View Source" and copy the css.

Paste the css into the following file:
``` /imports/stylesheets/vendor/loader.css ```




## Display the loader in the list while waiting for data

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



``` /imports/components/lists/list.jsx ```

```js
...
import { Loader } from '../loader/loader.jsx'

export const List = (props) =>{

   ...
	return  props.subsReady?
   ...
    :
    <Loader />
}

...

```



