# Add a React Data Container

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
import { App } from '../components/app'

export default createContainer(
	() => {
		
		const
		  noteId = FlowRouter.getParam('_id'),
		  sub = Meteor.subscribe('note.details', noteId),
			note = sub.ready()? Note.findOne({_id: noteId }) : {},
			handleUpdates = (collection, field, value) =>  {		
		    const doc = {}
		    doc[field] = value
		    collection.set(doc)
		    Meteor.call('/note/save', collection, (err, result) => AppLib.db.handleDbResult(err, result))
			}

	  return {
		  note,
		  subsReady: sub.ready(),
		  handleUpdates
	  }
  },
  AppLayout
)
```




