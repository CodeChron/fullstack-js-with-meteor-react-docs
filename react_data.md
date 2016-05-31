# Handle Data (Containers)

We'll use the [container component](https://medium.com/@learnreact/container-components-c0e67432e005#.5se1cppmo) model for handling data in React.

## Add packages
Next, let's add support for using data in a React/Meteor app. There are [many ways](https://www.discovermeteor.com/blog/data-loading-react/) to do this.  I prefer the [official](http://guide.meteor.com/react.html) Meteor method.

- Add the Meteor package: ```meteor add react-meteor-data```
- Add the npm package: ```npm i react-addons-pure-render-mixin --save```


## Create a container and data handler 
Let's create a container component that will handle data for our homepage container.

``` /imports/components/containers/homepage_container.js ```

```js
import { createContainer } from 'meteor/react-meteor-data'
import { Note } from '../../collections/notes'
import { Meteor } from 'meteor/meteor'
import { App } from '../app'

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
	  	handleSubmit: handleCreateNote
	  }
  },
  App
)
```


## Update our homepage route to use the data container and pass along props from the container into a page

``` /imports/routes.jsx ```

```js

```

## Install a utility for viewing db info in the browser

Rather than have to keep opening a Mongo console, let's install a utlity for doing this directly in the browser.  [Mongol](https://github.com/msavin/Mongol) is great for this.

Simply type ``` meteor add msavin:mongol ```
After that, you can use Ctrl + m to display an in-browser console.


## Notes - move to later section
Here we will create a single "Controller" component for managing:
- Creating a note
- Listing existing notes

Add a NotesContainer
**Render NotesContainer as a top-level component (this is new from the preview version, we need to do this because we want to have a top-level container structure to handle component state for the entire page)**

(when we add users, we'll switch this to users container)



