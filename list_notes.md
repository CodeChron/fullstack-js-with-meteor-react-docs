# List Notes

Now that we've created some data, let's display it.


## Create a List Component

``` /imports/components/lists/list.jsx ```

```js
import React from 'react'

export const List = (props) =>
  <ul className="list-group">
    { 
    	props.collection.map((item) =>
    		<li key={item._id} className="list-group-item">
    		  {item.title}
    		</li>
      )
	   }
  </ul>

List.propTypes = {
	collection: React.PropTypes.array.isRequired
}
```


## Pass notes data from the container into the homepage

Our list requires an array of data to display.  Let's make that available via our data container.

``` /imports/components/containers/homepage_container.js ```

```js
import { createContainer } from 'meteor/react-meteor-data'
import { Note } from '../../collections/notes'
import { App } from '../app'

export default createContainer(
	() => {
		
		const 
		  notes = Note.find().fetch()
		  ,
		  ...

	  return {
	  	collection: notes,
	  	...
	  }
  },
  App
)
```

## Add it to our homepage

## Display notes data in the list
