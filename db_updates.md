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
('notes' here matches what you type when you created the Mongo collection in ``` /imports/api/notes/notes.jsx ```)
  
## Create a data schema

- What is a data schema and should we have one?
- What are some options for creating a schema in Meteor?


## Add a title field

## Remove the "insecure" package (do not allow create on the client side)
## Update db calls to use Meteor.call and Meteor.methods
## Remove the 'autopublish' package

# Add publications and subscriptions

# Add a loading component



