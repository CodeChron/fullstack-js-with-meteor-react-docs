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

## 'Turn on' the link feature





