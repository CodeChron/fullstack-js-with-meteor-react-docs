# Add a content editor
Next, let's create a simple for updating note content.  This form will autosave content that is typed into it.

``` imports/components/forms/autosave_input.jsx ```

```js
import React from 'react'
import debounce from 'lodash.debounce'

export class AutoSaveInput extends React.Component {

  constructor(props){
    super(props)
    this.state = {
      contentValue: this.props.contentValue
    }
  }

  handleUpdates(updatedValue){

    const
      updateInterval = 250,
      options = { 'maxWait': 2000 },
      submitUpdates = (collection, field, value) => {
        this.props.handleUpdates(collection, field, value)
      },
      autoSaveChanges = debounce(submitUpdates, updateInterval, options)
      
    autoSaveChanges(this.props.note, this.props.field, updatedValue)
  }

  handleOnChange(e) {
    const updatedValue = e.target.value
    this.setState({contentValue: updatedValue})
    this.handleUpdates(updatedValue)
  }

  handleOnBlur() {
    this.props.doneEditing()
  }

  render() {
    return  <form>
              <div className="form-group">
                <textarea
                  className="form-control"
                  placeholder={this.props.placeholder}
                  value={this.state.contentValue}
                  onChange={this.handleOnChange.bind(this)}
                  onBlur={this.handleOnBlur.bind(this)}
                />
              </div>
              <button className="btn btn-default">Done</button>
            </form>
  }
}

AutoSaveInput.propTypes = {
  handleUpdates: React.PropTypes.func.isRequired,
  field: React.PropTypes.string.isRequired,
  contentValue: React.PropTypes.string,
  placeholder: React.PropTypes.string
}

AutoSaveInput.defaultProps = {
  contentValue:  ""  ,
  placeholder: "Write something..."
}
```

TODO: discuss everything going on in this component.
- adding the debounce utility
- handle changes to the textarea
- handle blur and relationship to autofocus

Note that we need to add autoFocus. In order for a blur event to be triggered, the input needs to be in focus. (Blur is the opposite of focus.  If the field is not in focus, blur will not do anything.)

Here, we are "wrapping" the ``` ContentEditor ``` component in a ``` EditableContent ``` component that has a  ```editMode``` state.  Clicking on the content block will switch the state to edit mode.  Then, we are passing a callback prop to the ContentEditor that will toggle edit mode again when the form input is blurred.

## Dummy Done button

Also, note that the Done button in the ContentEditor is really just a dummy and serves no actual function.  It doesn't matter if we click on it or not.  However, it can improve the user experience to provide something to click on to exit edit mode.


## Add the content editor to the note details page

``` /imports/components/pages/note_details_page.jsx ```

```js
...
import { ContentEditor } from '../forms/content_editor'

export const NoteDetailsPage = (props) => {
  ...
            <div id="main-content" className="container">
             <ContentEditor content={props.note.content} />
            </div>
  ...
}
```

Here, we are inserting the content editor and passing in the new content field we just created.

You should now see the (still not yet functional) content editor component when viewing a note details page.


## TODO: Add support for updating content in the container and provide a db method

```js
...

Meteor.methods({

  ...
  ,
	'note.update': (note) => {
			note.set({
			  updatedAt: new Date()
			})
			note.save()
			return note
  }
...
})
```


```js
export default createContainer(
	() => {
		
		const
        ...
			,
			handleUpdateNote = (collection, field, value) => {
			  const doc = {}
			  doc[field] = value
			  collection.set(doc)
		
	      Meteor.call('note.update', collection, (err, result) => {
          if (err) {
            console.log('error: ' + err.reason)
          }
        })
		  }

	  return {
	  	note,
	  	handleUpdateNote,
	  	subsReady: sub.ready()
	  }
  },
  App
)
```

## Notes - integrate this

# Autosave Content Changes

Now, let's actually update note content when typing into this component

## Add a db method for updating notes
We first need to add a server side operation for handling note updates.

``` /imports/collections/notes.js ```

```js
...
Meteor.methods({

   ...

	'/note/update': (note) => {

		note.set({
		  updatedAt: new Date()
		})
		note.save()
		return note
  },
  ...
})
```


## Add a handler for saving content changes

Now we can use this handler in our data container, and add a function that components can call and pass in content updates.

``` /imports/components/containers/note_details_container.js ```

```js
...
export default createContainer(
	() => {
		
		const
         ...
			,
			handleUpdateNote = (note, field, value) => {
			  const doc = {}
			  doc[field] = value
			  note.set(doc)
		
		      Meteor.call('/note/update', note, (err, result) => {
	            if (err) {
	              console.log('error: ' + err.reason)
	            }
	          })
		    }
	  
	  return {
          ...
		  handleUpdates: handleUpdateNote
	  }
  },
  App
)
```

This function will allow  us to use the same handler both for note content and the note title, as well as any future note fields we might add.

## Pass props to the content editor

We now need to pass along the necessary props to the content editor component. 

``` /imports/components/pages/note_details_page.jsx ```

```js
...

export const NoteDetailsPage = (props) => {
  ...
	  ,
	  noteContent = props.subsReady? <ContentEditor contentValue={props.note.content} field={"content"} {...props} /> :   <Loader />
 ...
```

## Update content editor to auto-save changes

``` /imports/components/forms/content_editor.jsx ```

```js
...
import React from 'react'
import debounce from 'lodash.debounce'

export class ContentEditor extends React.Component {

  ...

  handleUpdates(updatedValue){

    const
      updateInterval = 250,
      options = { 'maxWait': 1000 },
      submitUpdates = (collection, field, value) => {
        this.props.handleUpdates(collection, field, value)
      },
      debouncedUpdates = debounce(submitUpdates, updateInterval, options)
      
     debouncedUpdates(this.props.note, this.props.field, updatedValue)
  }

  handleOnChange(e) {
    const updatedValue = e.target.value
    this.setState({contentValue: updatedValue})
    this.handleUpdates(updatedValue)
  }

	render() {
    
    return  <form>
              <div className="form-group">
                <textarea
                 ...
                  onChange={this.handleOnChange.bind(this)}
                />
              </div>
              ...
            </form>
	}
}

ContentEditor.propTypes = { 
  contentValue: React.PropTypes.string.isRequired,
  field: React.PropTypes.string.isRequired,
  handleUpdates: React.PropTypes.func.isRequired
}
...
```

We made quite a few updates to this component.  You'll need to install the [lodash.debounce](https://www.npmjs.com/package/lodash.debounce) utility for this to work.

``` npm i lodash.debounce --save ```

Debounce is great for handling events that are fired in rapid succession.  In our case, these are the keypress events that are fired every time the user types a key when updating note content.  We are using this utility to only submit a keypress event every 250ms, and to wait up to 1000ms (one second) to submit the keypress event if they are fired continuously.





