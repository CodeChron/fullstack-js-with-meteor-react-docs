# Db Updates: Add a Title Field

Why add a title field?

## Remove existing notes
Why remove existing notes before adding a new field?

With the Meteor server running, open a new terminal window and type:
  ```meteor mongo ```
  This will open up Meteor's built-in db console.
  Then remove all items from the Notes collection:
  
  ```
  ⚡   meteor mongo
MongoDB shell version: 2.6.7
connecting to: 127.0.0.1:3001/meteor
meteor:PRIMARY> db.notes.remove({})
WriteResult({ "nRemoved" : 1 })
meteor:PRIMARY> 
```


## Add a title field to the note schema

``` /imports/api/notes/notes.js ```
```js
...

const NoteSchema = Class.create({
	name: 'Note',
	collection: Notes,
	fields: {
    title: String,
    ...
  }
})
```


## Update the notes list to use the title instead of content

## Set content to have a default value on create


