# Make db operations secure

Currenlty, if you open the Mongol utility we just added, you'll notice that it is possible to insert and update db data directly from the browser.  This is very insecure and we do not want to allow this in a production app.

## Remove the "insecure" package
First, we need to remove a package that Meteor includes by default, to allow for exactly the type of work we just did earliery, quickly getting dat handling up and running.  Now, however, let's update this to be more a "real" way of handling db data.  The first step is to remove this package.

```  meteor remove insecure ```

Now, if you try creating a note, you'll notice it doesn't work.  This is because we need to move these operations to the server side.

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

