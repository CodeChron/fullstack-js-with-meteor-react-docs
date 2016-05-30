# Add Styling with Bootstrap

We'll now install our first Meteor package.

Make sure you are in your app directory:
``` meteor add twbs:bootstrap ```

After you install this, you should see the default welcome screen with Bootstrap styling applied.
![Default welcome screen with Bootstrap installed](https://raw.githubusercontent.com/CodeChron/fullstack-js-preview-docs/master/images/bootstrap-dflt.png)


## Add mobile meta tags (optional)
This helps ensure proper rendering on mobile devices.
In Meteor, there is no need to include the html tags.  Meteor will do that for you.

``` /client/head.html ```

```html
<head>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
</head>
```