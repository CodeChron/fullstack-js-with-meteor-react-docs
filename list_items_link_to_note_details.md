# List items link to note details

Now that we've added a details page, we want to be able to access it. Let's turn the note titles in the list on our homepage into links to the detail view.

## Update lists items to include a link

For this, we will first import our router into the list component, then add list items that link as an optional feature of the list. 

``` /imports/components/lists/list.jsx ```

```js
...
import { FlowRouter } from 'meteor/kadira:flow-router'

export const List = (props) => {

	 const listFeatures = {
  	linkItem: (item) => <a href={FlowRouter.path(props.linkRoute , {_id: item._id})}>{item.title}</a>  	
	}
  
  const displayList = props.collection.map((item) => 
    	<li key={item._id}>
    	  {props.linkItem? 
	 	      listFeatures.linkItem(item)
	 	      :
	 	       item.title
	 	    }
    	  <DeleteBtn handleDelete={props.handleDeleteNote} itemToDelete={item} />
    	</li>)
    ...
}

List.propTypes = {
	...
	linkItem: React.PropTypes.array.bool,
    linkRoute: React.PropTypes.array.string
}

List.defaultProps = {
	linkItem: false,
    linkRoute: null
}
```

## Refactoring opportunity: Make delete an optional feature

Now that we've made linking list items an optional feature, let's use the opportunity to also make deleting of items optional. This will make our list more reuasable.

```js
import React from 'react'
import { FlowRouter } from 'meteor/kadira:flow-router'
import { DeleteBtn }  from '../buttons/delete_btn'

export const List = (props) => {

	 const listFeatures = {
  	linkItem: (item) => <a href={FlowRouter.path(props.linkRoute , {_id: item._id})}>{item.title}</a>,
  	deleteItem: (item) => <DeleteBtn handleDelete={props.handleDeleteNote} itemToDelete={item} />	
	}
  
  const displayList = props.collection.map((item) => 
    	<li key={item._id}>
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
    	</li>)

  return <ul>{displayList}</ul>
}

List.propTypes = {
	collection: React.PropTypes.array.isRequired,
	linkItem: React.PropTypes.array.bool
}

List.defaultProps = {
	linkItem: false,
	deleteItem: false
}
```

## 'Turn on' desired list features

Now, since, these are optional feature, we'll need to turn them on.  Let's do that in our container.

``` /imports/components/containers/homepage_container.js ```

```js
export default createContainer(() => {
 ...
 return {
   ...
   deleteItem: true,
   linkItem: true,
   linkRoute: "noteDetails"
  }
}, App)
```


## Pop quiz: how would we make the new item form optional?
Similar to item links, not all lists will want to have a new item form.  How would we make that feature optional?


