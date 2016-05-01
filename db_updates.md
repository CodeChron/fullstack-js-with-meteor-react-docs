# Db updates

## Clear out existing notes

With the Meteor server running, open a new terminal window and type:
  ```meteor mongo ```
  This will open up Meteor's built-in db console.
  Then remove all items from the Notes collection:
  
  ```
  ⚡   meteor mongo
MongoDB shell version: 2.6.7
connecting to: 127.0.0.1:3001/meteor
meteor:PRIMARY> db.notes.remove({})
WriteResult({ "nRemoved" : 1 })
meteor:PRIMARY> 
```
  ('notes' here matches what you type when you created the Mongo collection in ``` /imports/api/notes/notes.jsx ```
  
  
- create a data schema
- add a title field
- remove insecure (do not allow create on the client side)
- update db calls to use Meteor.call and Meteor.methods
- remove autopublish
- add pub/sub and loadin comp


