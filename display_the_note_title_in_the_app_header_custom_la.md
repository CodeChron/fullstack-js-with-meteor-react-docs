# Display the note title in the note detail header (custom layout)

Next, we want to display the title for the current note in the header (instead of the app name)

## Add a note details publication

``` /imports/collections/server/publications.js ```

```js
...

const
  ...
  noteDetailsFields = {
    title: 1
  }

...

Meteor.publish('note.details', function(id) {
  return Note.find({_id: id }, { fields: noteDetailsFields})
})
```

Here, we are requiring a specific id in order to subscribe to the publication. Also, note that even though we only want one note document, we don't use  ``` findOne ``` since a publication needs to return a cursor and cannot return a specific document.

## Create a note details container

Since we are now going to be using data on this page, we need to wrap it in a container.


``` /imports/componenents/containers/note_details_container.js ```

```js
import { createContainer } from 'meteor/react-meteor-data'
import { FlowRouter } from 'meteor/kadira:flow-router'
import { Note } from '../../collections/notes'
import { App } from '../app'


export default createContainer(
	() => {
		
		const
		  noteId = FlowRouter.getParam('_id'),
		  sub = Meteor.subscribe('note.details', noteId),
			note = sub.ready()? Note.findOne({_id: noteId }) : {}

	  return {
		  note
	  }
  },
  App
)
```

Here, we are first getting the id for this note via the url (getParam.)  Then, we are subscribing to and finding the specific note once the subscribtion is ready. Finally, we are returning the note object, making it available for use in child components.

 ## Update our route to use the note details container

Earlier, we made use of the Homepage containter to display our placeholder note details page.  Now that we need to display actual note details data, let's replace it with the note details container we created.

``` /imports/routes.jsx ```

```js

```
 
 
 ## Display the note title

- Wrap into single_column_layout
- pass note id param into route
- get note title using route :_id param






