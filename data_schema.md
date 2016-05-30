# Data Schema

Let's model our note data and also add some other helpers for making data handling easier.

## Install a data model layer

There are many ways to do this.  We'll use the [Astronomy](https://github.com/jagi/meteor-astronomy/) package.

```  meteor add jagi:astronomy ```

## Add a data schema

- What is a data schema and why should we have one?
- What are some options for creating a schema in Meteor?

``` /imports/collections/notes.js ```

```js
...
import { Class } from 'meteor/jagi:astronomy'

const Notes = new Mongo.Collection('notes')

export const Note = Class.create({
	name: 'Note',
	collection: Notes,
	fields: {
    title: String,
    updatedAt: Date 
  }
})
```

Note that we are now exporting our Note schema rather than the Notes collection.  Let's now also be sure to import the schema rather than collection to the server:

``` /imports/startup/server/index.js ```

```js 
import { Note } from '/imports/collections/notes'
```




