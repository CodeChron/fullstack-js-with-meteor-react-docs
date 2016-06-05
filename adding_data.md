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

Note that we are now exporting our *Note* schema rather than the Notes collection. 

## Make db operations secure

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

Note that we also are returning the note that we created, so that we, among other things, can let the client side know if there were any errors in completing the operation.


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