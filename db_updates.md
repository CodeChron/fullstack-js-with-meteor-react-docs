# DB Updates
_possibly separate these out into individual chapters_

# Only allow db operations on the server side

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


# Add a title field


## Remove existing notes

With the Meteor server running, open a new terminal window and type:
  ```meteor mongo ```
  This will open up Meteor's built-in db console.
  Then remove all items from the Notes collection:
  
  ```
  ⚡   meteor mongo
MongoDB shell version: 2.6.7
connecting to: 127.0.0.1:3001/meteor
meteor:PRIMARY> db.notes.remove({})
WriteResult({ "nRemoved" : 1 })
meteor:PRIMARY> 
```
('notes' here matches what you type when you created the Mongo collection in ``` /imports/api/notes/notes.jsx ```)


## Require explicit publish and subscribe

## Remove the 'autopublish' package

## Add publications and subscriptions

# Add a loading component



