# List items link to note details

Now that we've added a details page, we want to be able to access it. Let's turn the note titles in the list on our homepage into links to the detail view.

## Update lists items to include a link

For this, we will first import our router into the list component, and then update the list item text to be a link.

``` /imports/components/lists/list.jsx ```

```js
...
import { FlowRouter } from 'meteor/kadira:flow-router'

export const List = (props) => {
	return props.subsReady? <ul className="list-group">
    <li className="list-group-item">
      <SingleFieldSubmit {...props} />
    </li>
    { 
    	props.collection.map((item) =>
    		<li key={item._id} className="list-group-item">
    		   <a href={FlowRouter.path( "noteDetail" , {_id: item._id})}>{item.title}</a> 
           ....
    		</li>
          ...
```

## This code will work, but is this good code?