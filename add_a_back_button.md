# Add a Back Button

This next step is very similar to what we did with the DeleteBtn we created. You should try first doing this on your own and then comparing with the steps below.

## Add back button icon
We only want this back button to appear on the note details page.  Let's therefore pass it into that instance of the AppHeaderLayout.

 ``` /imports/components/pages/note_details_page.jsx ```

```js
...
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
	  headerCenter = props.subsReady? <PageTitle title={props.note.title} /> : null
	  
	return  <div id="app-container">
            <AppHeaderLayout headerLeft={backBtn} headerCenter={headerCenter} />
            ...
          </div>
}
```

- create an IconBtn component
- use glyphicon glyphicon-menu-left, see http://getbootstrap.com/components/
- add IconBtn with back icon to appHeader in note details view
- on click of back btn, do history.back
