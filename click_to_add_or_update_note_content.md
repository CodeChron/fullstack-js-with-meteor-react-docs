# Add note content

Next, we are going to allow for adding note content.  These are our general requirements for this feature:

- Allow for clicking on note content to edit it.
- Content auto-saves as you type.
- Click on Done or anywhere outside the content to exit edit mode. (discuss: the done button is just a dummy.)
- Display a message if there is no content. (That also is clickable so you can add content.)
- (Later, we'll add the ability to also exit edit mode by using Shift + Return.)

## Add content to our Note model
First, we need to add somewhere to store content.

``` /imports/collections/notes.js ```

```js
...
export const Note = Class.create({
  ...
	fields: {
    ...
    content: {
      type: String,
      default: ''
    }
  }
})
...
```

Here we are adding a content field that has a default value of an empty string. By adding this field to our data model, we will now also be able to access and update the content field in our components using ``` props.notes.content ```

## Remove existing notes
Because we have updated our data model, you should clear out current notes, so that you'll be working with notes that have this field.

```
$ meteor mongo
MongoDB shell version: 2.6.7
connecting to: 127.0.0.1:4001/meteor
meteor:PRIMARY> db.notes.remove({})
WriteResult({ "nRemoved" : 3 })
meteor:PRIMARY> exit
bye
```

## Update our note details publication
Next, we need to make sure we also publish this new field to the note details page.

``` /imports/collections/server/publications.js ```

```js
...
const
  ...
  noteDetailsFields = {
    ...
    content: 1
  }
...
```

## Add a content editor component we can use for the note details view

We need a component that allows for editing content.  Let's add that now.

``` /imports/components/forms/content_editor.jsx ```

```js
import React from 'react'

export class ContentEditor extends React.Component {

	constructor(props) {
    super(props);
    this.state = {
      content: this.props.content
    }
  }

	render() {

    return  <form>
              <div className="form-group">
                <textarea
                  className="form-control"
                  placeholder={this.props.placeholder}
                  value={this.state.content}
                />
              </div>
              <button className="btn btn-default">Done</button>
            </form>
	}
}

ContentEditor.propTypes = { 
  content: React.PropTypes.string
}

ContentEditor.defaultProps = {
  content: "",
  placeholder: "Write something..."
}
```

This is a basic form that doesn't actually do anything yet.  However, it's easier to work with something we can actually play with, so let's  render it into our page, and then make it funcional (eg. save content entered into the form.)

## Add the content editor to the note details page

``` /imports/components/pages/note_details_page.jsx ```

```js
...
import { ContentEditor } from '../forms/content_editor'

export const NoteDetailsPage = (props) => {
  ...
            <div id="main-content" className="container">
             <ContentEditor />
            </div>
  ...
}
```

You should now see the content editor component when viewing a note details page.

## Autosave changes


## Exit edit mode and display





