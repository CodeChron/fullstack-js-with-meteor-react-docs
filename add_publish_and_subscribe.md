# Add Publish and Subscribe

Why is this step needed?

## Remove the 'autopublish' package

```  meteor remove autopublish ```

You should now see that there are no notes being displayed.

![no-notes-published](https://cloud.githubusercontent.com/assets/819213/14958482/03118ef2-1059-11e6-855b-8e7d9f5cb9f7.png)

Our notes are still in the database.  However, we now need to explicitly publish and subscribe to our notes.

## Add a Notes Publication

``` /imports/api/notes/server/publications.js ```

```js
import { Meteor } from 'meteor/meteor'
import { Notes } from '../notes'

const
  notesListFields = {
    title: 1,
    updatedAt: 1
  }

Meteor.publish('notes.all', function() {
  return Notes.find({}, { fields: notesListFields})
})
```

Why the server directory?
Why only publish certain fields?

## Import to the server on startup
 ``` /imports/startup/server/index.js ```
 ```js 
 ...
 import '../../api/notes/server/publications'
 ```
 

## Subscribe to the notes publication

# Add a loading component



