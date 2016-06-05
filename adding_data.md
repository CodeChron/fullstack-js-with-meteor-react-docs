# Adding Data

Before we can start creating notes, we need to add support for use of data, both in Meteor generally and also specifically for React within Meteor.

## Create a Mongo dB Notes Collection
``` /imports/collections/notes.js ```

```js
import { Mongo } from 'meteor/mongo'

const Notes = new Mongo.Collection('notes')
```

# Add a data model layer
Let's model our note data and also add some other helpers for making data handling easier.

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

Note that we are now exporting our *Note* schema rather than the Notes collection.  Let's now also be sure to import the schema rather than collection to the server:

## Make the collection available on the server

The Notes collection we created needs to be available on the server side for database operations to work.

``` /imports/startup/server/index.js ```

```js 
import { Note } from '/imports/collections/notes'
```

``` /server/main.js ```

```js 
import '/imports/startup/server/'
```

## Try inserting and removing some dummy data

We'll use the Mongo shell that ships with Meteor.

```
$ meteor mongo
MongoDB shell version: 2.6.7
connecting to: 127.0.0.1:4001/meteor
meteor:PRIMARY> db.notes.insert({foo: "bar" })
WriteResult({ "nInserted" : 1 })
meteor:PRIMARY> db.notes.find()
{ "_id" : ObjectId("574c81991fdb1f15bfbf0655"), "foo" : "bar" }
meteor:PRIMARY> db.notes.remove({})
WriteResult({ "nRemoved" : 1 })
meteor:PRIMARY> exit
bye
```