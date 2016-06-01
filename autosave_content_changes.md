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
      }

    this.autoSave = this.autoSave || debounce(submitUpdates, updateInterval, options)

    this.autoSave(this.props.note, this.props.field, updatedValue)
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

We made quite a few updates to this component.

