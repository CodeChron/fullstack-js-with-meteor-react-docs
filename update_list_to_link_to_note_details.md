# Update List to link to note details

_TODO: update previous branches with this refactoring of optional feature?

## Add listLink as an optional list feature

- change the title field to be a link
- get the path using FlowRouter
- make listLinks an optional feature


Will mean either:

```
{item.title}
```
or 
```
<a href={FlowRouter.path( "noteDetail" , {_id: item._id})}>{item.title}</a>
```




