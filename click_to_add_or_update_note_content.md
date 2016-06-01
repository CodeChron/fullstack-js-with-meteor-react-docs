# Add note content

Next, we are going to allow for adding note content in the note details page.  First, we need to add somewhere to store content and then publish the new field where it will be used.


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




