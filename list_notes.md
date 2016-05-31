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

## Add it to our homepage

## Display notes data in the list
