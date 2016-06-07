# Click to edit the note title

We want to make it possible to edit the note title. This feature will be very similar to what we did when implementing the main content editor. You should try building this on your own first and then compare it to the steps below.

One thing to keep in mind: in contrast to note content, a note title can't be empty. (If it were, then there would be nothing to click on in the notes list.)  Also, while the content block can be multiple lines (textarea) the title will just be a single line (text input)

- create a click to edit component, with only a single line field option for now
- add a handler for updating the title
- update title on keypress
- exit click to edit on return/submit or blur


This will be the most advanced component created so far.


``` note_details_page.jsx ```

```js
import React from 'react'
import { AppHeader } from '../layouts/app_header'
import { PageTitle } from '../content/page_title'
import { ContentBlock } from '../content/content_block'
import { ClickToEdit } from '../forms/click_to_edit'
import { EditablePageTitle } from '../content/editable_page_title'
import { EditableContent } from '../content/editable_content'
import { LoadingFeedback } from '../utility/loading_feedback'
import { IconBtn } from '../buttons/icon_btn'

export const NoteDetailsPage = (props) => {

	const
    handleBackBtnClick = () => history.back()
    ,
    backBtn = <IconBtn icon="arrow_back" handleClick={handleBackBtnClick} />
    ,
	  displayEditableComponent = (clickableComponent, contentValue, field, multiline) => <ClickToEdit
	      clickableComponent={clickableComponent}
	      field={field}
	      contentValue={contentValue}
	      handleUpdates={props.handleUpdateNote}
	      multiline={true}
	      {...props}
	    />

  if (props.subsReady){

  	const
  	  noteTitleComponent = <PageTitle title={props.note.title} />,
	    noteContentComponent = <ContentBlock contentValue={props.note.content} />

	  return <div id="app-container" className="l-app-full-height l-app-centered">
           <AppHeader leftCol={backBtn} middleCol={displayEditableComponent(noteTitleComponent, props.note.content, "title", false)} />
           <div id="main-content">
           {displayEditableComponent(noteContentComponent, props.note.content, "content", true)}
           </div>
         </div>

  } else {
     return <div id="app-container" className="l-app-full-height l-app-centered">
             <LoadingFeedback />
           </div>
  }
}
```


``` click_to_edit.jsx ```

```js
import React from 'react'
import { AutoSaveInput } from './auto_save_input'

export class ClickToEdit extends React.Component {

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
      <AutoSaveInput doneEditing={this.toggleEditMode.bind(this)} {...this.props} />
      :
      <span className="clickable" onClick={this.toggleEditMode.bind(this)}>{this.props.clickableComponent}</span> 
  }
}

ClickToEdit.propTypes = { 
  clickableComponent: React.PropTypes.object.isRequired,
  editMode: React.PropTypes.bool
}

ClickToEdit.defaultProps = {
  editMode: false
}
```



