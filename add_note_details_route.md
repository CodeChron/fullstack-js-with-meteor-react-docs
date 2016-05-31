# Add Note Details Route


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

## Add a note details placeholder page


## Test the route

Try inserting a note id into your url. You can get this from the Mongo console or from Mongol.

eg. ``` http://localhost:3000/notes/NdNvRmEj2Rus3Nt3o ```  

If everything is hooked up properly you should see a default app header with no content.