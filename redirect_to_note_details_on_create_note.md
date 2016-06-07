# Redirect to Note Details when creating a Note

Next, when creating a new note, let's automatically display the note details, making it easy to start adding detailed content. (We haven't added detailed content yet, but we'll get to that in a bit.)

## Add a redirect function and call it when a note is created

This is something we'll want to handle in our Homepage container.

``` /imports/components/containers/homepage_container.jsx ```

```js
 ...
import { FlowRouter } from 'meteor/kadira:flow-router'

export default createContainer(() => {

	const
    ...
    ,
    redirectToNoteDetails = (note) => FlowRouter.go("noteDetails", {_id: note._id})
    ,
    handleCreate = (title) => {
      Meteor.call('/note/create', title, (err, result) => {
      if (err) {
	      console.log('error: ' + err.reason)
	   } else {
	      redirectToNoteDetails(result)
	   }
      })
  	},
    
    handleDelete = (note) => {
  		Meteor.call('/note/delete', note._id, (err, result) => {
        if (err) {
          console.log('there was an error: ' + err.reason)
        }
      })
  	}
   ...

```

First, we added a function for handling a redirect.  Then, we called the function if there were no error in creating the note.


