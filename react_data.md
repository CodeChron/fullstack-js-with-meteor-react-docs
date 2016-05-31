# Handle Data (Containers)

We'll use the [container component](https://medium.com/@learnreact/container-components-c0e67432e005#.5se1cppmo) model for handling data in React.

## Add packages
Next, let's add support for using data in a React/Meteor app. There are [many ways](https://www.discovermeteor.com/blog/data-loading-react/) to do this.  I prefer the [official](http://guide.meteor.com/react.html) Meteor method.

- Add the Meteor package: ```meteor add react-meteor-data```
- Add the npm package: ```npm i react-addons-pure-render-mixin --save```


## Create a Homepage data container
Let's create a container component that will handle data for our homepage container.

``` /imports/components/containers/homepage_container.js ```

```js
import { createContainer } from 'meteor/react-meteor-data'
import { Note } from '../collections/notes'
import { Meteor } from 'meteor/meteor'
import { App } from '../app'

export default createContainer(
	() => {
		
		const
		  sub = Meteor.subscribe('notes.list.all'),
			notes = sub.ready()? Note.find({}, { sort: { updatedAt: -1 }}).fetch() : []
			,
			redirectToNoteDetail = note => FlowRouter.go("noteDetail", {_id: note._id})
			,
		  handleCreateNote = (title) => {
		    Meteor.call(
		    	'/note/create',
		    	title,
		    	(err, result) => AppLib.db.handleDbResult(err, redirectToNoteDetail(result))
		    )
		  }
		  ,
		  handleDeleteNote = (note) => {
		  	Meteor.call(
		  		'/note/delete',
		  		note._id,
		  		(err, result) => handleDbResult(err)
        )
	    }

	  return {
		  notes,
		  subsReady: sub.ready(),
	  	handleSubmit: handleCreateNote,
	  	handleDelete: handleDeleteNote,
		  placeholder: "New Note",
		  addItem: true,
      deleteItem: true,
      linkItem: true
	  }
  },
  App
)
```




## Notes - move to later section
Here we will create a single "Controller" component for managing:
- Creating a note
- Listing existing notes

Add a NotesContainer
**Render NotesContainer as a top-level component (this is new from the preview version, we need to do this because we want to have a top-level container structure to handle component state for the entire page)**

(when we add users, we'll switch this to users container)



