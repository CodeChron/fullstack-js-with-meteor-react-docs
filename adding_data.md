# Adding Data

Before we can start creating notes, we need to add support for use of data, both in Meteor generally and also specifically for React within Meteor.

## Create a Mongo dB Notes Collection
``` /imports/collections/notes/notes.js ```

```js
import { Mongo } from 'meteor/mongo'

export const Notes = new Mongo.Collection('notes')
```

## Make the collection available on the server

The Notes collection we created needs to be available on the server side for database operations to work.

``` /imports/startup/server/index.js ```

```js 
import { Notes} from '/imports/api/notes/notes'
```

``` /server/index.js ```

```js 
import '/imports/startup/server/'
```

## Try inserting some data

_show insertion of a 'foo' field with a 'bar' value_


## Next: Add a Data Schema

