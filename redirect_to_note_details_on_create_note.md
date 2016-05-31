# Redirect to Note Details when creating a Note


_Also, refactored createContainer while adding this..._

``` /imports/components/containers/notes_container.jsx ```

```js
 ...
import { FlowRouter } from 'meteor/kadira:flow-router'

export default createContainer(() => {

	const
    sub = Meteor.subscribe('notes.all'),

    notes = sub.ready()? Notes.find({}, { sort: { updatedAt: -1 }}).fetch() : [],

    //add this
    redirectToNoteDetail = note => FlowRouter.go("noteDetail", {_id: note._id}),

    handleCreate = (title) => {
      Meteor.call('/note/create', title, (err, result) => {
        if (!err) {
          //call it here
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
   ...

```


