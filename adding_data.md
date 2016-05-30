# Adding Data

Before we can start creating notes, we need to add support for use of data, both in Meteor generally and also specifically for React within Meteor.

## Create a Mongo dB Notes Collection
``` /imports/collections/notes.js ```

```js
import { Mongo } from 'meteor/mongo'

export const Notes = new Mongo.Collection('notes')
```

## Make the collection available on the server

The Notes collection we created needs to be available on the server side for database operations to work.

``` /imports/startup/server/index.js ```

```js 
import { Notes} from '/imports/collections/notes'
```

``` /server/main.js ```

```js 
import '/imports/startup/server/'
```

## Try inserting some data

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