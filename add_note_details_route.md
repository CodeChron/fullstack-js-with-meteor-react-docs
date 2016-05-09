# Add Note Details Route


## Add a placeholder note details route

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

- Try inserting a note id into your url, eg. ``` http://localhost:3000/notes/NdNvRmEj2Rus3Nt3o ```
-  You can get this from the Mongo console.

If everything is hooked up properly you should see just a default app header with no content.