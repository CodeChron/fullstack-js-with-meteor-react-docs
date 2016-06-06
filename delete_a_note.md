# Delete a Note

Next, let's add the ability to delete a note.

## Add an Icon Button component
We're going to need a button with a delete icon that accepts a click event. 

## Link to an icon library

We'll use Google's [Material Design Icons](https://design.google.com/icons/).  Here we will simply link to the icon font.  However, for a production app, you are strongly recommended to either store the icons on your server or use service such as IcoMoon
Let's first create a generic icon button.

``` /imports/components/buttons/icon_btn.jsx ```

```js
import React from 'react'

export const IconBtn = (props) =>
  <button
    onClick={props.handleClick}
    className="btn btn-default btn-xs"
    title={props.title}
     alt={props.title}
  >
    <span className={props.icon} aria-hidden="true"></span>
  </button>

IconBtn.propTypes = {
	handleClick: React.PropTypes.func.isRequired,
	icon: React.PropTypes.string.isRequired,
	title: React.PropTypes.string
}
```

 ## Add a DeleteBtn Component
 Next, let's use our IconBtn to create a button specifically for deleting stuff.
 
 ``` /imports/components/buttons/delete_btn.jsx ```
 
```js
import React from 'react'
import { IconBtn } from './icon_btn'

export const DeleteBtn = (props) => {

  const handleDelete = (item) => {
    const confirmDelete = confirm(props.confirmMsg)

    if (confirmDelete) {
      props.handleDelete(item)
    }
  }

  return <IconBtn handleClick={()=> handleDelete(props.itemToDelete)}
            title={props.title}
            alt={props.title}
            icon={props.icon}
          />
}

DeleteBtn.propTypes = {
  itemToDelete: React.PropTypes.object.isRequired,
	handleDelete: React.PropTypes.func.isRequired,
  confirmMsg: React.PropTypes.string,
}

DeleteBtn.defaultProps = {
  icon: "glyphicon glyphicon-remove",
  title: "Delete...",
  confirmMsg: "Really delete this?"
}
```

Why the need for ```{() => callFunction()}```? See http://stackoverflow.com/questions/33846682/react-onclick-fuction-fires-on-render (TL;DR "Because you want to pass a call to the function rather than the function directly.")
 
 
## Add Support for deleting notes

First, we need to add a delete method on the server side.

``` /imports/collections/notes.js```

```js
...

Meteor.methods({
...
,
  '/note/delete': (id) => Note.remove(id)
})

```

Next, we add a delete handler to our container.

``` /imports/containers/homepage_container.jsx ```

```js
...

export default createContainer(() => {
 ...
	  ,
	  handleDeleteNote = (note) => {
			Meteor.call('/note/delete', note._id, (err, result) => {
        if (err) {
          console.log('error: ' + err.reason)
        }
      })
	  }		  
  return {
    ...
	  handleDelete: handleDeleteNote,
  }

}, App)
```

## Add DeleteBtn to the List


``` /imports/components/lists/list.jsx ```

```js
...
import { DeleteBtn } from '../buttons/delete_btn'

export const List = (props) =>{
   ...
	    	props.collection.map((item) => {
            return <li key={item._id} className="list-group-item">{item.content}
	 	      <span className="pull-right"><DeleteBtn itemToDelete={item} {...props} /></span>
	 	      </li>
           ...
```

## Handle deletion of a note



- How are we able to write just ```handleDelete```?

You should now have a delete feature included with each list item:

<img width="829" alt="delete-feature" src="https://cloud.githubusercontent.com/assets/819213/15051636/a3a2997c-12c7-11e6-8c11-4f70f163d8b8.png">