# Setting up your project

Create your project directory and cd into it:

```mkdir my_notes_app  && cd my_notes_app```

Add a [ReadMe](https://en.wikipedia.org/wiki/README) file to your project: 
```echo "# My Notes App" >> README.md```

(Optional, [initialize a git repo](https://help.github.com/articles/adding-an-existing-project-to-github-using-the-command-line/).)


## Create the app
Create the Meteor app and cd into it:

```meteor create app``

``` cd app```

## Start the app and get set up for development

- Type ``` meteor ``` to run the app locally.
- Go to http://localhost:3000 to view your app.
- Make sure you have both your client console and your server log visible while working. See [this blog post](http://coderchronicles.org/2016/04/08/getting-started-with-meteor-1-3-react-and-flowrouter/#Start_and_View_Meteor_in_Your_Browser) for a recommended local setup.
- Open up a new terminal tab/window.
- Open up your project in an editor of your choice.

## Remove default app content

Before continuing, let's remove the default files and content created by Meteor.

Remove ``` main.css ```
Clear out all content in ``` main.js ``` and ``` main.html ```


## Move app files into the "imports" directory"

After the release of Meteor 1.3, it's generally considered good practice to place the majority of your files inside the imports directory and then load them as needed. [Learn more](http://guide.meteor.com/structure.html#javascript-structure) in the Meteor Guide.

- Create an imports directory, and a client-side directory for files we want to load on startup: ``` mkdir -p imports/startup/client ```
- Move files from ``` /client ``` to ```/imports/startup/client ```: ``` mv /client/main* /imports/startup/client` ``` 
- Add a "import manifest" file: 

``` /imports/startup/client/index.js ```

```js
import './main.html'
import './main.js'
```

- Import all files in the import manifest to the client:

``` /client/main.js ```

```js
import '/imports/startup/client'
```

You should now just see a blank white page.  That's ok.  It just means everything is working but nothing is being rendered to the screen.


