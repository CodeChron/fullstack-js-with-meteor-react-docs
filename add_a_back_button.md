# Add a Back Button

This next step is very similar to what we did with the DeleteBtn we created. You should try first doing this on your own and then comparing with the steps below.

These are your requirements:
_todo: think of earlier places where I can start to take this approach.  Also, consider having the requirements on a separate page and then link to this page for the answer_

We only want this back button to appear on the note details page. 

 ``` /imports/components/pages/note_details_page.jsx ```

```js
...
import { IconBtn } from '../buttons/icon_btn'

export const NoteDetailsPage = (props) => {

	const
    handleBackBtnClick = () => history.back(),
    backBtn = <IconBtn
              icon="arrow_back"
              handleClick={handleBackBtnClick}
            />
    ,
	...

  return <div id="app-container" className="l-app-full-height l-app-centered">
           <AppHeader leftCol={backBtn} middleCol={pageTitle} />
           <div id="main-content">
           {noteContent()}
           </div>
         </div>	
}
