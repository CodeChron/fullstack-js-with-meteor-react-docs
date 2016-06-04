# Move app files into the "imports" directory"

After the release of Meteor 1.3, it's generally considered good practice to place the majority of your files inside the imports directory and then load them as needed. [Learn more](http://guide.meteor.com/structure.html#javascript-structure) in the Meteor Guide.

- Create an imports directory, and a client-side directory for files we want to load on startup: ``` mkdir -p imports/startup/client ```
- Move files from ``` /client ``` to ```/imports/startup/client ```: ``` mv /client/main* /imports/startup/client` ``` 
- Add a "import manifest" file: 

``` /imports/startup/client/index.js ```

```js
import './main.css'
import './main.js'
```

- Import all files in the import manifest to the client:

``` /client/main.js ```

```js
import '/imports/startup/client'
```

You should now see the same welcome info you see by default.  If so, you know your imports are set up properly.

## Remove default app content

Before continuing, let's remove the default files and content created by Meteor.



## Discussion Topics
 - Lazy vs eager loading.
 - Import/Export and the module system.