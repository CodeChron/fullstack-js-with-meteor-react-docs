# Redirect to Note Details on Create Note

# On create of a note, redirect to note details page


```js
import { createContainer } from 'meteor/react-meteor-data'
import { Notes } from '../../api/notes/notes'
import { List } from '../lists/list'
import { Meteor } from 'meteor/meteor'
import { FlowRouter } from 'meteor/kadira:flow-router'

export default createContainer(() => {

	const
    sub = Meteor.subscribe('notes.all'),

    notes = sub.ready()? Notes.find({}, { sort: { updatedAt: -1 }}).fetch() : [],

    redirectToNoteDetail = note => FlowRouter.go("noteDetail", {_id: note._id}),

    handleCreate = (title) => {
      Meteor.call('/note/create', title, (err, result) => {
        if (!err) {
          redirectToNoteDetail(result)
        } else {
          console.log('there was an error: ' + err.reason)
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

  return {
  	handleSubmit: handleCreate,
	  placeholder: "New Note",
	  subsReady: sub.ready(),
	  collection: notes,
	  handleDelete: handleDelete,
	  addItem: true,
	  deleteItem: true,
    linkItem: true
  }

}, List)

```

## Lock the note count before creating to prevent the note from appearing before redirect

_TODO: do this later - premature optimization?
- get note count before create
- set list limit to current count - via subscription
- reset limit after redirect


