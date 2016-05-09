# Update List to link to note details
- change the title field to be a link
- get the path using FlowRouter
- make listLinks an optional feature: linkItem: false, linkPath


Will mean either:

```
{item.title}
```
or 
```
<a href={FlowRouter.path( "noteDetail" , {_id: item._id})}>{item.title}</a>
```




