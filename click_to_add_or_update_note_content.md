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
        this.displayContent(this.props.contentValue)   
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

Discuss what we did above, eg that we are checking for if content is empty.

## Add the EditableContent component to the note details page and display note content

``` imports/components/pages/note_details_page.jsx ```

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



