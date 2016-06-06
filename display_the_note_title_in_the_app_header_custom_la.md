# Display note detail content


Next, we want to display note details content.  More specifically, we want to display the note title in the header and note content in the main content block.


- add a content field to the Note schema
- add note details publication
- create a data container for note details to subscribe to and handle note detail data

title for the current note in the header (instead of the app name)

## Add content to our Note model

``` /imports/collections/notes.js ```

```js
...
export const Note = Class.create({
  ...
	fields: {
    ...
    content: {
      type: String,
      default: ''
    }
  }
})
...
```

Here we are adding a content field that has a default value of an empty string. By adding this field to our data model, we will now also be able to access and update the content field in our components using ``` props.notes.content ```

## Remove existing notes
Because we have updated our data model, you should clear out current notes, so that you'll be working with notes that have this field.

```
$ meteor mongo
MongoDB shell version: 2.6.7
connecting to: 127.0.0.1:4001/meteor
meteor:PRIMARY> db.notes.remove({})
WriteResult({ "nRemoved" : 3 })
meteor:PRIMARY> exit
bye
```

## Update our note details publication
Next, we need to make sure we also publish this new field to the note details page.

``` /imports/collections/server/publications.js ```

```js
...
const
  ...
  noteDetailsFields = {
    ...
    content: 1
  }
...
```


## Add a note details publication

``` /imports/collections/server/publications.js ```

```js
...

const
  ...
  noteDetailsFields = {
    title: 1
  }

...

Meteor.publish('note.details', function(id) {
  return Note.find({_id: id }, { fields: noteDetailsFields})
})
```



Here, we are requiring a specific id in order to subscribe to the publication. Also, note that even though we only want one note document, we don't use  ``` findOne ``` since a publication needs to return a cursor and cannot return a specific document.

## Create a note details container

Since we are now going to be using data on this page, we need to wrap it in a container.


``` /imports/componenents/containers/note_details_container.js ```

```js
import { createContainer } from 'meteor/react-meteor-data'
import { FlowRouter } from 'meteor/kadira:flow-router'
import { Note } from '../../collections/notes'
import { App } from '../app'


export default createContainer(
	() => {
		
		const
		  noteId = FlowRouter.getParam('_id'),
		  sub = Meteor.subscribe('note.details', noteId),
			note = sub.ready()? Note.findOne({_id: noteId }) : {}

	  return {
		  note,
          subsReady: sub.ready()
	  }
  },
  App
)
```

Here, we are first getting the id for this note via the url (getParam.)  Then, we are subscribing to and finding the specific note once the subscribtion is ready. Finally, we are returning the note object, and a prop we can use to check if subs are ready, making them available for use in child components.

 ## Update our route to use the note details container

Earlier, we made use of the Homepage containter to display our placeholder note details page.  Now that we need to display actual note details data, let's replace it with the note details container we created.

``` /imports/routes.jsx ```

```js
...
import NoteDetailsContainer from './components/containers/note_details_container'
...

FlowRouter.route('/notes/:_id', {
 ...
  action() {
    mount(NoteDetailsContainer, {
...
})

```
 
 
 ## Display the note title
 
 ``` /imports/components/pages/note_details_page.jsx ```

```js
...

export const NoteDetailsPage = (props) => {

	const
	  headerCenter = props.subsReady? <PageTitle title={props.note.title} /> : null
	  
	return  <div id="app-container">
            <AppHeaderLayout headerCenter={headerCenter} />
            ...
          </div>
}
```

We need to wait for subscriptions to be ready before displaying the page title.  Here, we are simply displaying nothing until the subscription is ready. In the future we might want to update this to display a loading indicator.





