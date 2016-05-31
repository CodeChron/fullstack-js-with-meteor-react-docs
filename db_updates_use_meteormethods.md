# Make db operations secure

Currenlty, if you open the Mongol utility we just added, you'll notice that it is possible to insert and update db data directly from the browser.  This is very insecure and we do not want to allow this in a production app.

## Remove the "insecure" package
First, we need to remove a package that Meteor includes by default, to allow for exactly the type of work we just did earlier, quickly getting dat handling up and running.  Now, however, let's update this to be more a "real" way of handling db data.  The first step is to remove this package.

```  meteor remove insecure ```

[Learn more here](http://docs.meteor.com/api/collections.html#Mongo-Collection-allow).

## Move db operations to server side


## Update db calls Meteor.methods

``` /imports/collections/notes.js ```

```js
...
import { Meteor } from 'meteor/meteor'
...
Meteor.methods({

	'/note/create': (title) => {
      const note = new Note()
			note.set({
			  title,
			  updatedAt: new Date()
			})
			note.save()
			return note
  }

})
```

## "Call" the method from our data container

``` /imports/components/containers/homepage_container.js ```

```js
...
import { Meteor } from 'meteor/meteor'
...

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
	  	handleSubmit: handleCreateNote
	  }
  },
  App
)
```

