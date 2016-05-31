# Publish and Subscribe to Notes

Currently we are automatically publishing data from the server.  That's fine for prototyping purposes, but for a real app, we want control what is published and what the client subscribes to.

## Remove the 'autopublish' package

```  meteor remove autopublish ```

You should now see that there are no notes being displayed.

![no-notes-published](https://cloud.githubusercontent.com/assets/819213/14958482/03118ef2-1059-11e6-855b-8e7d9f5cb9f7.png)

Our notes are still in the database.  However, we now need to explicitly publish and subscribe to our notes.

## Add a Notes Publication

``` /imports/collections/server/publications.js ```

```js
import { Meteor } from 'meteor/meteor'
import { Notes } from '../notes'

const
  notesListFields = {
    title: 1,
    updatedAt: 1
  }

Meteor.publish('notes.all', function() {
  return Notes.find({}, { fields: notesListFields})
})
```

Why the server directory?
Why only publish certain fields?

## Import to the server on startup

 ``` /imports/startup/server/index.js ```
 ```js 
 ...
 import '/imports/collections/server/publications'
 ```
 
## Subscribe to the notes publication


``` /imports/components/containers/homepage_container.js ```

```js
export default createContainer(() => {

	const sub = Meteor.subscribe('notes.all')

	const notes = sub.ready()? Notes.find({}, { sort: { updatedAt: -1 }}).fetch() : []
  
   ...
   
  return {
   ...
	  subsReady: sub.ready(),
   ...
  }

}, List)
```


``` /imports/components/lists/list.jsx ```

```js
export const List = (props) =>{

...
	
	return props.subsReady?
	  <ul className="list-group">
	    {displayFeature(props.addItem, listFeatures.addItem)}
	    { 
	    	props.collection.map((item) => {
	 	      return <li key={item._id} className="list-group-item">{item.title} {displayFeature(props.deleteItem, listFeatures.deleteItem, item)}
	 	      </li>
	      })
	    }
    </ul>
    :
    null
}

List.propTypes = {
	...
	subsReady: React.PropTypes.bool.isRequired,
	...
}
...
```


