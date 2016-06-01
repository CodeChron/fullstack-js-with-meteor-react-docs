# Display Note Content View Mode

We now want to be able to exit the content editor and display our notes in view mode.  Additionally, we want to be able to click on the viewer to return to edit mode.

## Add a content viewer
First, let's create a component we can use to view note content.

``` /imports/components/content/editable_content.jsx ```

```js
import React from 'react'
import { ContentEditor } from '../forms/content_editor'
import '../../stylesheets/helpers.css'

export class EditableContent extends React.Component {

  constructor(props){
    super(props)
    this.state = {
      editMode: this.props.editMode
    }
  }

  toggleEditMode(){
    this.setState({ editMode: !this.state.editMode })
  }

  render() {
      return this.state.editMode?
        <ContentEditor doneEditing={this.toggleEditMode.bind(this)} {...this.props}  />
      :
        <span className="clickable" onClick={this.toggleEditMode.bind(this)}>{this.props.contentValue}</span>
        
  }
}

EditableContent.propTypes = { 
  contentValue: React.PropTypes.string.isRequired,
  editMode: React.PropTypes.bool
}

EditableContent.defaultProps = {
  editMode: false
}
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

## Display a clickable message if there is no content

What if I delete all content?  I will still need something to click on so I can add content.


## Use Shift + Return to exit edit mode



