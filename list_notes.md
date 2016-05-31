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

## Add the list to the homepage

``` /imports/components/pages/homepage.js ```

```js
...
import { List } from '../lists/list'
...

export const Homepage = (props) => {

  ...

	return  <div id="app-container">
            ...
            <div id="main-content" className="container">
              <SingleFieldSubmit
                placeholder={"New Note..."}
                handleSubmit={props.handleSubmit}
              />
             <List collection={props.collection} />
            </div>
          </div>
}
```

You should now see notes appear in your browser. 

## Display new notes on top

If you try adding notes, you'll see they appear in the list. However, they are appearing at the bottom of the list. Let's make them appear on top.  We'll do that by changing the sort order in which notes are returned.

``` /imports/components/containers/homepage_container.js ```

```js
 ...

export default createContainer(
	() => {
		...
		const 
		  notes = Note.find({}, { sort: { updatedAt: -1}}).fetch()
		  ,
		  ...
)
```


## Move the new note form into the list component

Let's also move the new note form into the list component.  This will both look a little nicer and later we'll be able to make this an optional feature of a list.




