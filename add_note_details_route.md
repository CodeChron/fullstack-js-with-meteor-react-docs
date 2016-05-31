# Add Note Details Route

## Add a note details placeholder page

``` /imports/components/pages/note_details.jsx ```

```js
import React from 'react'
import { AppHeaderLayout } from '../layouts/app_header_layout'
import { EditableText } from '../content/editable_text'
import { IconBtn } from '../buttons/icon_btn'

export const NoteDetailsPage = (props) => {

	const
    handleBackBtnClick = () => history.back(),
    backBtn = <IconBtn
                icon="glyphicon glyphicon-menu-left"
                btnSize="btn-lg"
                handleClick={handleBackBtnClick}
              />
    ,
    pageTitleBlock = <h1 className="navbar-brand full-width">{props.note.title}</h1>
    ,
    noteContentBlock = <p>FOO</p>
    ,
    editablePageTitle = <EditableText
                          viewBlock={pageTitleBlock}
                          editableText={props.note.title}
                          field={"title"}
                          {...props}
                        />
    

	return <div id="app-container">
	  <AppHeaderLayout
	  headerLeft={backBtn}
	  headerCenter={editablePageTitle}
	  />
	  <div id="main-content" className="container">
	    <EditableText
                          viewBlock={noteContentBlock}
                          editableText={""}
                          field={"content"}
                          multiline={true}
                          {...props}
                        />
	  </div>
	</div>
}

```

## Add a note details route

``` /imports/startup/client/routes.jsx ```

```js
FlowRouter.route('/notes/:_id', {
  name: 'noteDetail',
  action(params) {
    mount(AppLayout, {
      header: () => <AppHeaderLayout />,
      content: () => null
    })
  }
})
```

Here we are displaying a default app header and no content, just as a placeholder for our page.




## Test the route

Try inserting a note id into your url. You can get this from the Mongo console or from Mongol.

eg. ``` http://localhost:3000/notes/NdNvRmEj2Rus3Nt3o ```  

If everything is hooked up properly you should see a default app header with no content.