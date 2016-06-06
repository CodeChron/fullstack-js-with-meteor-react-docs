# Add a content editor
Next, let's create a simple content editor component and insert it into the note details page.



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

Here, we are "wrapping" the ``` ContentEditor ``` component in a ``` EditableContent ``` component that has a  ```editMode``` state.  Clicking on the content block will switch the state to edit mode.  Then, we are passing a callback prop to the ContentEditor that will toggle edit mode again when the form input is blurred.

## Toggle Edit Mode when the ContentEditor form blurs

Note that we need to add autoFocus. In order for a blur event to be triggered, the input needs to be in focus.

Also, note that the Done button in the ContentEditor is really just a dummy and serves no actual function.  It doesn't matter if we click on it or not.  However, it can improve the user experience to help make it clear how to exit edit mode.

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

We need a component that allows for editing content.  Let's add that now.

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


