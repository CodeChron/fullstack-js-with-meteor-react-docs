# Publish and Subscribe to Notes


**TODO: wait for all subs to be ready, need a better loading strategy  - might be a blog post and something I can use for the talk**

Currently we are automatically publishing data from the server.  That's fine for prototyping purposes, but for a real app, we want control what is published and what the client subscribes to.

## Remove the 'autopublish' package

```  meteor remove autopublish ```

You should now see that there are no notes being displayed.

![no-notes-published](https://cloud.githubusercontent.com/assets/819213/14958482/03118ef2-1059-11e6-855b-8e7d9f5cb9f7.png)

Our notes are still in the database.  However, we now need to explicitly publish and subscribe to our notes.

## Add a Notes Publication

``` /imports/collections/server/publications.js ```

```js
import { Meteor } from 'meteor/meteor'
import { Notes } from '../notes'

const
  notesListFields = {
    title: 1,
    updatedAt: 1
  }

Meteor.publish('notes.list', function() {
  return Notes.find({}, { fields: notesListFields})
})
```

TODO: change meteor methods naming convention to be consistent.
TODO: discuss - why are we not using arrow function?

Why the server directory?
Why only publish certain fields?

## Import to the server on startup

 ``` /imports/startup/server/index.js ```
 ```js 
 ...
 import '/imports/collections/server/publications'
 ```
 
## Subscribe to the notes publication

``` /imports/components/containers/homepage_container.js ```

```js
export default createContainer(() => {

	const 
	  sub = Meteor.subscribe('notes.list')
	  ,
	  notes = sub.ready()? Note.find({}, { sort: { updatedAt: -1}}).fetch() : []
	 ,
   ...
}, App)
```

TODO: explain sub.ready() and use of empty array

Your notes should now re-appear in your browser.



