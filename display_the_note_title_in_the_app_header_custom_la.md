# Display the note title in the note detail header (custom layout)

Next, we want to display the title for the current note in the header (instead of the app name)

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

## Create a note details container

Since we are now going to be using data on this page, we need to wrap it in a container.  

 ## Update our route to use the note details container
 
 
 ## Display the note title

- Wrap into single_column_layout
- pass note id param into route
- get note title using route :_id param






