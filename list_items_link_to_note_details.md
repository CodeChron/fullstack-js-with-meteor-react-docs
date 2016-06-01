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

## Let's make links optional

With links now being a required feature of this component, this makes the omponent less reusable, ie I cannot use this component in situations where maybe I just want to list stuff that doesn't link.

Therefore, let's make this an optional feature.  This will require first moving the feature into a feature collection, and then adding a prop that allows us to turn "on" the feature.

```js
...

export const List = (props) => {

 const listFeatures = {
  	linkItem: (item) => <a href={FlowRouter.path(props.linkRoute , {_id: item._id})}>{item.title}</a>  	
	}
    
	return props.subsReady? <ul className="list-group">
    <li className="list-group-item">
      <SingleFieldSubmit {...props} />
    </li>
    { 
    	props.collection.map((item) =>
    		<li key={item._id} className="list-group-item">
            {props.linkItem? 
	 	       listFeatures.linkItem(item)
	 	      :
	 	        item.title
	 	     }
           ....
    		</li>
          ...
 List.propTypes = {
	linkItem:  React.PropTypes.bool,
    linkRoute: React.PropTypes.string
}

List.defaultProps = {
	linkItem: false
}
```

Now, since, this is an optional feature, we'll need to turn it on when we use it.  Let's do that in our container.

``` /imports/components/containers/homepage_container.js ```

```js
export default createContainer(() => {
 ...
 return {
   ...
   linkItem: true,
   linkRoute: "noteDetails"
  }
}, App)
```


## Pop quiz: how would we make the new item form optional?
Similar to item links, not all lists will want to have a new item form.  How would we make that feature optional?


