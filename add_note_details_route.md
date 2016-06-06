# Add Note Details Page

## Create a note details placeholder page

``` /imports/components/pages/note_details.jsx ```

```js
import React from 'react'
import { AppHeader } from '../layouts/app_header'
import { PageTitle } from '../content/page_title'

export const NoteDetails = (props) => {

	const
	  pageTitle = <PageTitle title={"Note Details"} />

  return <div id="app-container" className="l-app-full-height l-app-centered">
           <AppHeader middleCol={pageTitle} />
           <div id="main-content">
           {"Note details content."}
           </div>
         </div>	
}
```


## Add a note details route

``` /imports/routes.jsx ```

```js
...
import { NoteDetailsPage } from './components/pages/note_details_page'

FlowRouter.route('/notes/:_id', {
  name: 'noteDetails',
  action() {
    mount(HomepageContainer, {
      content: (props) => <NoteDetailsPage {...props} />
    })
  }
})
```

Notice that we are using the HomepageContainer.  This is temporary until we actually start passing data into the note details page, at which point we'll create its own container.


## Test the route

Try inserting a note id into your url. You can get this from the Mongo console or from Mongol.

eg. ``` http://localhost:3000/notes/NdNvRmEj2Rus3Nt3o ```  

If everything is hooked up properly you should see the placeholder page we created.