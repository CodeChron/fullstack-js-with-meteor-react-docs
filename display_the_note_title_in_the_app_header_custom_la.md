# Display the note title in the note detail header (custom layout)

Next, we want to display the title for the current note in the header (instead of the app name)

## Add a note details publication

``` /imports/components/containers/homepage_container.jsx ```

```js
 ...
import { FlowRouter } from 'meteor/kadira:flow-router'

   ...

```

## Create a note details container

Since we are now going to be using data on this page, we need to wrap it in a container.  

 ## Update our route to use the note details container
 
 
 ## Display the note title

- Wrap into single_column_layout
- pass note id param into route
- get note title using route :_id param






