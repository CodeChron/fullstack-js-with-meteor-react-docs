# Add a Back Button

- create an IconBtn component
- use glyphicon glyphicon-menu-left, see http://getbootstrap.com/components/
- add IconBtn with back icon to appHeader in note details view
- on click of back btn, do history.back


This next step is very similar to what we did with the DeleteBtn we created. You should try first doing this on your own and then comparing with the steps below.

## A back button icon
We only want this back button to appear on the note details page.  Let's therefore pass it into that instance of the AppHeaderLayout.

 ``` /imports/components/pages/note_details_page.jsx ```

```js
...

export const NoteDetailsPage = (props) => {

	const
	  headerCenter = props.subsReady? <PageTitle title={props.note.title} /> : null
	  
	return  <div id="app-container">
            <AppHeaderLayout headerCenter={headerCenter} />
            ...
          </div>
}
```