# Add Note Details Route


## Add a placeholder note details route

``` /imports/startup/client/routes.jsx ```

```js
FlowRouter.route('/notes/:_id', {
  name: 'noteDetail',
  action(params) {
    mount(AppContainer, {
      header: () => <AppHeaderLayout />,
      content: null
    })
  }
})

```
