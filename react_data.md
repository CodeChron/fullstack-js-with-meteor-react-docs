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
import { Meteor } from 'meteor/meteor'
import { Note } from '../../collections/notes'
import { App } from '../app'

export default createContainer(
	() => {
		
		const handleCreateNote = (title) => {
		  Meteor.call('/note/create', title, (err, result) => {
            if (err) {
              console.log('error: ' + err.reason)
            }
          })
		}

	  return {
	  	handleCreateNote,
        placeholder: "New Note..."
	  }
  },
  App
)
```


The call method has three parts:
- Which db method are we using?
- What data are we passing in?
- How should we handle the result of the db operation?

Here we are "wrapping" the App component in a container for handling data.  We only have one handler so far, for creating a new note.

## Update our homepage route to use the data container and pass along props from the container into a page

We now need to update our route to use this wrapping container rather than the app component directly.

``` /imports/routes.jsx ```

```js
...
import HomepageContainer from '/imports/components/containers/homepage_container'

FlowRouter.route('/', {
  name: 'homepage',
  action() {
    mount(HomepageContainer, {
      page: (props) => <Homepage {...props} />
    })
  }
})
```
(Also, note that you can remove the ```App`` import since that is being passed in through ```HomepageContainer```

Note that we are _not_ using curly braces for the ``` HomepageContainer ```. This is because it uses ``` export default ``` meaning only one item is exported from the file, and we can name it on import.  We could have named this ``` BaconContainer ``` if we wanted.
