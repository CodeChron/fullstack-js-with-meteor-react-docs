# Db Updates: Use Meteor.methods


## Remove the "insecure" package
```  meteor remove insecure ```

Why is this step needed? Important?



## Update db calls to use Meteor.call and Meteor.methods


``` /imports/api/notes/notes.js ```
```js
...
import { Meteor } from 'meteor/meteor'
...
Meteor.methods({

	'/note/create': (content) => {
		const note = new NoteSchema()
    note.set({
      content: content,
      updatedAt: new Date()
    })

    note.save()
    return note
  },

  '/note/delete': (id) => Notes.remove({_id: id})

})

```

``` /imports/components/collections/notes_container.js ```

```js
...
import { Meteor } from 'meteor/meteor'
...

export default createContainer(() => {

	...

	const handleCreate = (content) => {
    Meteor.call('/note/create', content, (err, result) => {
      if (!err) {
        console.log('note: ' + result._id)
      } else {
        console.log('there was an error: ' + err.reason)
      }
    })
	}

	const handleDelete = (note) => {
	  Meteor.call('/note/delete', note._id, (err, result) => {
      if (err) {
        console.log('there was an error: ' + err.reason)
      }
    })
	}

...
}, List)


```

