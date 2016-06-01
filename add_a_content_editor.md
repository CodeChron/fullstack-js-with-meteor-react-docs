# Add a content editor
Next, let's create a simple content editor component and insert it into the note details page.

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
             <ContentEditor content={props.note.content} />
            </div>
  ...
}
```

Here, we are inserting the content editor and passing in the new content field we just created.

You should now see the (still not yet functional) content editor component when viewing a note details page.


