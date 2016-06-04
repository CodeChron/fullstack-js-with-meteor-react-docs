# Add React and React Router

Starting with Meteor 1.3, we install React packages using npm.

```meteor npm install react react-dom --save```

- Why are we using npm instead of a Meteor package?
- What is the react-dom package?
- ["What is the --save option for?"](http://stackoverflow.com/questions/19578796/what-is-the-save-option-for-npm-install) 


## Add a render target

``` /imports/startup/client/main.html ```

```html
<body>
  <div id="react-root"></div>
</body>
```

This will be our "render target" for React components.


## Create a routes file


## Import the routes file