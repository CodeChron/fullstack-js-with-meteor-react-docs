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

## Add btn size attribute

You might have noticed that we added a ```btnSize``` attribute, so that we, for example, can have larger buttons in the app header.  Let's update the IconBtn component to support that.

``` /imports/components/buttons/icon_btn.jsx ```

```js
...
import classNames from 'classNames'

export const IconBtn = (props) => {

  const btnClasses = classNames("btn icon-btn", props.btnSize)
  
  return <button
    ...
    className={btnClasses}
    ...
  >
    <span className={props.icon} aria-hidden="true"></span>
  </button>

}

IconBtn.propTypes = {
  ...
  btnSize: React.PropTypes.string
}

IconBtn.defaultProps = {
  btnSize: "btn-default"
}
```

Here, we are including the npm package [classNames](https://www.npmjs.com/package/classnames), so be sure to install it:

```meteor npm install classNames --save ```
