# Delete a Note


Next, let's add the ability to delete a note.

## Link to an icon library

We need access to a collection of icons for this and other UI controls.  We'll use Google's [Material Design Icons](https://design.google.com/icons/).  Here we will simply link to the icon font.  However, for a production app, you should consider either storing the icons on your server or using a service such as [IcoMoon](https://icomoon.io/).

``` /client/head.html ```

```html
<head>
  ...
  <link href="https://fonts.googleapis.com/icon?family=Material+Icons"
      rel="stylesheet">
</head>
```

## Create an Icon Button component
We're going to need a button with a delete icon that accepts a click event. Let's first create a generic icon button and then use that to create a delete button.

``` /imports/components/buttons/icon_btn.jsx ```

```js
import React from 'react'

export const IconBtn = (props) =>
  <button
    onClick={props.handleClick}
    title={props.title}
    alt={props.title}
  >
    <i className="material-icons">{props.icon}</i>
  </button>

IconBtn.propTypes = {
	handleClick: React.PropTypes.func.isRequired,
	icon: React.PropTypes.string.isRequired,
	title: React.PropTypes.string
}
```

 ## Create a DeleteBtn Component
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
  icon: "delete",
  title: "Delete...",
  confirmMsg: "Really delete this?"
}
```

Why the need for ```{() => callFunction()}```? See http://stackoverflow.com/questions/33846682/react-onclick-fuction-fires-on-render (TL;DR "Because you want to pass a call to the function rather than the function directly.")
 
 
## Add db handlers for deleting notes

Next, we need to add a delete method on the server side.

``` /imports/collections/notes.js```

```js
...

Meteor.methods({
...
,
  'note.delete': (id) => Note.remove(id)
})

```

Then, we add a delete handler to our container.

``` /imports/containers/homepage_container.jsx ```

```js
...

export default createContainer(() => {
 ...
 ,
 handleDeleteNote = (note) => {
   Meteor.call('note.delete', note._id, (err, result) => {
     if (err) {
       console.log('error: ' + err.reason)
      }
    })
  }
	  
  return {
    ...
	handleDeleteNote,
    ...
  }

}, App)
```

## Add DeleteBtn to the List


``` /imports/components/lists/list.jsx ```

```js
...
import { DeleteBtn }  from '../buttons/delete_btn'

export const List = (props) => {
  
  const displayList = props.collection.map((item) => 
    	...
    	   <DeleteBtn handleDelete={props.handleDeleteNote} itemToDelete={item} />
    	</li>)

...
}
...
```

##  Pass through delete props to the delete component

``` /imports/components/pages/homepage.jsx ```
```js
...
export const Homepage = (props) => {
 ...
	  displayList = () => props.subsReady? <List collection={props.notes} {...props} /> : <LoadingFeedback />
...
}
```



- How are we able to write just ```handleDelete```?

You should now have a delete feature included with each list item:

<img width="829" alt="delete-feature" src="https://cloud.githubusercontent.com/assets/819213/15051636/a3a2997c-12c7-11e6-8c11-4f70f163d8b8.png">