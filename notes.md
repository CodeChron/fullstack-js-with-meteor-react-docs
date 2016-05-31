# Notes


## Advanced New Note UI

The first feature we'll be implementing is a more advanced version of adding a new note.

_TODO: Insert mockups showing the advance version.

# Db Updates: Add a Title Field

Why add a title field?

## Remove existing notes
Why remove existing notes before adding a new field?

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


## Update note schema: Add a title field and a default value for content

``` /imports/api/notes/notes.js ```
```js
...
const NoteSchema = Class.create({
	name: 'Note',
	collection: Notes,
	fields: {
		title: String,
		content: {
      type: String,
      default: () => ""
    },
    updatedAt: Date 
  }
})
...
```

## Update note creation to use the title instead of content

``` /imports/components/containers/notes_container.js ```
```js
...
	const handleCreate = (title) => {
    Meteor.call('/note/create', title, (err, result) => {
     ...
	}
...
```

## Update the notes list to use the title instead of content


``` /imports/components/lists/list.jsx ```
```js
...
	return <ul className="list-group">
	   ...
	    { 
	    	props.collection.map((item) => {
	 	      return <li key={item._id} className="list-group-item">{item.title} 
              ...
	 	      </li>
	      })
	    }
...
```

# Update List to link to note details

_TODO: update previous branches with this refactoring of optional feature?

## Add listLink as an optional list feature

``` /imports/components/lists/list.jsx ```
```js
...
 const listFeatures = {
  ...
  	linkItem: (item) => <a href={FlowRouter.path( "noteDetail" , {_id: item._id})}>{item.title}</a>  	
	}
    
  ...
  	  <ul className="list-group">
	    {props.addItem?
	    	listFeatures.addItem()
	     :
	      null
	    }
	    { 
	    	props.collection.map((item) => {
	 	      return <li key={item._id} className="list-group-item">
	 	      {props.linkItem? 
	 	      	listFeatures.linkItem(item)
	 	        :
	 	        item.title
	 	       }
	 	        {props.deleteItem?
	 	        	listFeatures.deleteItem(item)
	 	         :
	 	          null
	 	        }
	 	      </li>
	      })
	    }
    </ul>
   ...
     
List.propTypes = {
   ...
	linkItem:  React.PropTypes.bool
}

List.defaultProps = {
    ...
	linkItem: false
}
```

You can test the link feature by temporarily setting ``` List.defaultProps ``` to ``` true ```.

## 'Turn on' the link feature

``` /imports/components/containers/notes_container.js ```
```js
...
  return {
    ...
    linkItem: true
  }
 ...
     
```




