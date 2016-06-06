# Display note content

Next, let's display note content.

We'll create an EditableContent component that display content in view mode by default and then switches to edit mode on click/tap.  if there is no content, we'll display a clickable message  Clicking on the message displays the content editor.

We now want to be able to exit the content editor and display our notes in view mode.  Additionally, we want to be able to click on the viewer to return to edit mode.

## Add an EditableContent component 
First, let's create a component we can use to view note content. This content will make the content editable by clicking/tapping on, hence the name.

``` /imports/components/content/editable_content.jsx ```

```js
import React from 'react'

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

  displayContent(contentValue){
    const
      noContentMsg = "Empty note.",
      isEmpty = contentValue === "",
      displayedContent = isEmpty? noContentMsg : contentValue

    return <span className="clickable" onClick={this.toggleEditMode.bind(this)}>{displayedContent}</span>
  }

  render() {
      return this.state.editMode?
        <div>{"Content editor placeholder"}</div>
      :
        {displayContent(this.props.note.content)}   
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

_is this the first actual React Component we've created?_

Discuss components and state.

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

What if I delete all content?  I will still need something to click on so I can add content.  Let's add content block that displays when there is no note content.

```js
...

export class EditableContent extends React.Component {
  ...
  handleViewContent(contentValue){

    const 
      noContentMsg = "Empty note.",
      isEmpty = contentValue === "",
      displayedContent = isEmpty? noContentMsg : contentValue

    return <span className="clickable" onClick={this.toggleEditMode.bind(this)}>{displayedContent}</span>
  }

  render() {
      return this.state.editMode?
        <ContentEditor doneEditing={this.toggleEditMode.bind(this)} {...this.props}  />
      :
        this.handleViewContent(this.props.contentValue)
        
  }
}
...
```


