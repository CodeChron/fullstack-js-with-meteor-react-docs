# Autosave Content Changes

Now, let's actually update note content when typing into this component

## Add a db method for updating notes
We first need to add a server side operation for handling note updates.

``` /imports/collections/notes.js ```

```js
...
Meteor.methods({

   ...

	'/note/update': (note) => {

		note.set({
		  updatedAt: new Date()
		})
		note.save()
		return note
  },
  ...
})
```


## Add a handler for saving content changes

Now we can use this handler in our data continer, and add a function that components can call and pass in content updates.

``` /imports/components/containers/note_details_container.js ```

```js
...
export default createContainer(
	() => {
		
		const
         ...
			,
			handleUpdateNote = (note, field, value) => {
			  const doc = {}
			  doc[field] = value
			  note.set(doc)
		
		      Meteor.call('/note/update', note, (err, result) => {
	            if (err) {
	              console.log('error: ' + err.reason)
	            }
	          })
		    }
	  
	  return {
          ...
		  handleUpdates: handleUpdateNote
	  }
  },
  App
)
```


## Autosave changes




## Exit edit mode and display

These are our general requirements for this feature:

- Allow for clicking on note content to edit it.
- Content auto-saves as you type.
- Click on Done or anywhere outside the content to exit edit mode. (discuss: the done button is just a dummy.)
- Display a message if there is no content. (That also is clickable so you can add content.)
- (Later, we'll add the ability to also exit edit mode by using Shift + Return.)


