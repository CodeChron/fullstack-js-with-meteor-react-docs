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


