# Notes

```

## Add btn size attribute

You might have noticed that we added a ```btnSize``` attribute, so that we, for example, can have larger buttons in the app header.  Let's update the IconBtn component to support that.

``` /imports/components/buttons/icon_btn.jsx ```

```js
...
import classNames from 'classNames'

export const IconBtn = (props) => {

  const btnClasses = classNames("btn icon-btn", props.btnSize)
  
  return <button
    ...
    className={btnClasses}
    ...
  >
    <span className={props.icon} aria-hidden="true"></span>
  </button>

}

IconBtn.propTypes = {
  ...
  btnSize: React.PropTypes.string
}

IconBtn.defaultProps = {
  btnSize: "btn-default"
}
```

Here, we are including the npm package [classNames](https://www.npmjs.com/package/classnames), so be sure to install it:

```meteor npm install classNames --save ```


``` imports/components/forms/content_editor.jsx ```

```js
...

export class ContentEditor extends React.Component {
 ...
	render() {

    return  <form>
              <div className="form-group">
                <textarea
                ...
                  autoFocus={true}
                  onBlur={this.props.doneEditing}
                />
              </div>
...
```

## Display a pointer cursor when (Desktop) users hover over editable content
This is also to improve UX.  We'll display a pointer when hovering over the content.

``` imports/stylesheets/helpers.css ```

```css
.clickable {
	cursor: pointer;
}
```


## Add a content editor component we can use for the note details view


``` /imports/components/forms/content_editor.jsx ```

```js
import React from 'react'

export class ContentEditor extends React.Component {

	constructor(props) {
    super(props);
    this.state = {
      contentValue: this.props.contentValue
    }
  }

	render() {

    return  <form>
              <div className="form-group">
                <textarea
                  className="form-control"
                  placeholder={this.props.placeholder}
                  value={this.state.contentValue}
                />
              </div>
              <button className="btn btn-default">Done</button>
            </form>
	}
}

ContentEditor.propTypes = { 
  contentValue: React.PropTypes.string
}

ContentEditor.defaultProps = {
  contentValue: "",
  placeholder: "Write something..."
}
```

This is a basic form that doesn't actually do anything yet.  However, it's easier to work with something we can actually play with, so let's  render it into our page, and then make it funcional (eg. save content entered into the form.)

## Move the new note form into the list component

Let's also move the new note form into the list component.  This will both look a little nicer and later we'll be able to make this an optional feature of a list.

First, remove ```SingleFieldSubmit ``` (both the import statement and the component) from the homepage and instead pass along the submitHandler into list.

``` /imports/components/pages/homepage.jsx ```

```js
...
export const Homepage = (props) => {
    ...

	return  <div id="app-container">
     ...
             <List collection={props.collection} handleSubmit={props.handleSubmit} placeholder={props.placeholder} />
    ...

```

Next, add it to the list component.

``` /imports/components/lists/list.jsx ```

```js
...
import { SingleFieldSubmit } from '../forms/single_field_submit'

export const List = (props) =>
  <ul className="list-group">
    <li className="list-group-item">
      <SingleFieldSubmit handleSubmit={props.handleSubmit} placeholder={props.placeholder} />
    </li>
    ...
  </ul>

 ...
```

## Refactor: use "copy props"

You'll notice that we are seeing ``` handleSubmit={props.handleSubmit} ``` in at least a couple places.  This means we are just passing along props from a parent component and is a "smell" that we can refactor this.

``` /imports/components/pages/homepage.jsx ```

```js
...
export const Homepage = (props) => {
    ...

	return  <div id="app-container">
     ...
             <List collection={props.collection} {...props} />
    ...

```

Next, add it to the list component.

``` /imports/components/lists/list.jsx ```

```js
...
import { SingleFieldSubmit } from '../forms/single_field_submit'

export const List = (props) =>
  <ul className="list-group">
    <li className="list-group-item">
      <SingleFieldSubmit {...props}  />
    </li>
    ...
  </ul>

 ...
```


We are using the "copy props" spread operator, which simply passes along all props from the parent.




## Add Custom Styling

``` /imports/stylesheets/main.scss ```

```scss
// VARIABLES
// ____________________________
$app-header-height: 5em;
$font-size-page-title: 1.5em;


// LAYOUTS
// ____________________________

.three-col-layout {
	display: flex;
  flex-flow: row nowrap;
  align-items:center;

  .main-content {
  	flex: 1;
  }
  
  .left-right-icons {
    width: 3em;
  }

}

// APP HEADER
// ____________________________

.navbar {
  height: $app-header-height;
 
  .navbar-header {

    .navbar-brand {
      margin-top: .25em;
      font-size: $font-size-page-title;
    }
  }
}

// BUTTONS
// ____________________________

.icon-btn{
  background-color: transparent;
  border:none;
  
  margin:0;
   padding:.5em;
  text-align: center;

  &:focus {
   outline: 0;
  }
}

// HELPERS
.centered {
  text-align: center;
}
.full-width {
  width: 100%;
}
```

## Importing existing stylesheets
Now that we are using sass, we should import all other stylesheets into the main stylesheets.  This will ensure that everything gets combined and minified into a single css file.

- Remove imports from: ``` /imports/components/loader/loader.jsx ``` and ``` /imports/components/content/editable_content.jsx ```
- Rename these files to ``` /imports/stylesheets/vendor/_loader.scss ``` and  ``` /imports/stylesheets/_helpers.scss ``` (prefixing the name with an underscore means it will only be loaded if explicitly imported.)
- Import them into the main scss file (add this to the end):

```scss
...
// HELPERS
// ____________________________
@import 'helpers';

// VENDOR
// ____________________________
@import 'vendor/loader';

```

## Import styles on startup
Note that this should be the first file to be loaded.

``` /imports/startup/client/index.js ```

```js
import '../../stylesheets/main'
...
```




## Update the app header to have a three column layout

``` /imports/components/layouts/app_header_layout.jsx ```

```js
...

export const AppHeaderLayout = (props) => {

	return <nav className="navbar navbar-default three-col-layout">
      <div className="left-right-icons">
	    {props.headerLeft}
	  </div>
	  <div className="container">
	    <div className="navbar-header">
	      {props.headerCenter}
	    </div>
	 </div>
      <div className="left-right-icons">
	    {props.headerRight}
	  </div>
	</nav>

}

AppHeaderLayout.propTypes = {
  headerLeft: React.PropTypes.object,
  headerCenter: React.PropTypes.object,
  headerRight: React.PropTypes.object
}

AppHeaderLayout.defaultProps = { 
  headerLeft: null,
  headerCenter: null,
  headerRight: null
}
```

Your page title should now appear as centered in the app header


## Exit edit mode and display

These are our general requirements for this feature:

- Allow for clicking on note content to edit it.
- Content auto-saves as you type.
- Click on Done or anywhere outside the content to exit edit mode. (discuss: the done button is just a dummy.)



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




