# Add note content with auto-save

Next, we are going to allow for adding note content.  These are our general requirements for this feature:

- Allow for clicking on note content to edit it.
- Content auto-saves as you type.
- Click on Done or anywhere outside the content to exit edit mode. (discuss: the done button is just a dummy.)
- Display a message if there is no content. (That also is clickable so you can add content.)
- (Later, we'll add the ability to also exit edit mode by using Shift + Return.)

## Add content to our Note model
First, we need to add somewhere to store content.

``` /imports/collections/notes.js ```

```js
...
export const Note = Class.create({
  ...
	fields: {
    ...
    content: String
  }
})
...
```

By adding this field to our data model, we will now also be able to access and update the content field in our components using ``` props.notes.content ```


## Add a content block to the note details view

## Autosave changes

## Exit edit mode and display





