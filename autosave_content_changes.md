# Autosave Content Changes

Now, let's actually update note content when typing into this component

These are our general requirements for this feature:

- Allow for clicking on note content to edit it.
- Content auto-saves as you type.
- Click on Done or anywhere outside the content to exit edit mode. (discuss: the done button is just a dummy.)
- Display a message if there is no content. (That also is clickable so you can add content.)
- (Later, we'll add the ability to also exit edit mode by using Shift + Return.)

## Add a handler for saving content changes

We need to add a function to our data container that components can call and pass content updates.

``` /imports/components/containers/note_details_container.js ```

```js
...
export default createContainer(
	() => {
		
		const handleCreateNote = (title) => {
			const note = new Note()
			note.set({
				title,
			  updatedAt: new Date()
			})
			note.save()
		  }

	  return {
	  	handleSubmit: handleCreateNote,
        placeholder: "New Note..."
	  }
  },
  App
)



## Autosave changes


## Exit edit mode and display

