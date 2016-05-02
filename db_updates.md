# DB Updates
_possibly separate these out into individual chapters_


# Add a title field


## Remove existing notes

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


## Require explicit publish and subscribe

## Remove the 'autopublish' package

## Add publications and subscriptions

# Add a loading component



