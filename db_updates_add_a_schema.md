# Db Updates: Add a Schema

 - Install the [Astronomy](https://github.com/jagi/meteor-astronomy/) package.

```  meteor add jagi:astronomy ```

## Add a data schema

- What is a data schema and why should we have one?
- What are some options for creating a schema in Meteor?

``` /imports/api/notes/notes.js ```
```js
...
import { Class } from 'meteor/jagi:astronomy'

const NoteSchema = Class.create({
	name: 'Note',
	collection: Notes,
	fields: {
    content: String,
    updatedAt: Date 
  }
})
```


