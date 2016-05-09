# Add a loading spinner

Let's display an animation instead of just null.
There are a ton of options for doing this.  

## Create a loader component


### Get the css
Go to http://projects.lukehaas.me/css-loaders/ and pick a loader of your choice.  Click on "View Source" and copy the css.

Paste the css into the following file:
``` /imports/stylesheets/vendor/loader.css ```


### Create the component

``` /imports/components/loader/loader.jsx ```

```js
import React from 'react'
import '../../stylesheets/vendor/loader.css'

export const Loader = () => <div className="loader">Loading...</div>
```


## Use the loader in the list component


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



