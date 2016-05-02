# Add a loading spinner

Let's display an animation instead of just null.
There are a ton of options for doing this.  

## Create a loader component


### Get the css
Go to http://projects.lukehaas.me/css-loaders/ and grab a loader of your choiced.  Click on "View Source" and copy the css.

Paste the css into the following file:
``` /imports/components/loader/loader.css ```


### Create the component

``` /imports/components/loader/loader.jsx ```

```js
import React from 'react'
import './loader.css'

export const Loader = () => <div className="loader">Loading...</div>
```


## Use the loader in the list component


``` /imports/components/lists/list.jsx ```

```js
import React from 'react'
import './loader.css'

export const Loader = () => <div className="loader">Loading...</div>
```



