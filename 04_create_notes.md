# Create notes

A good approach to implementing a feature is often to "follow the data."  Here, we'll do so first creating a form for creating a note and then adding a handler for inserting that note into our database.

_discuss why we'll first create a simpler form for creating notes and then add the more advanced UI later_

## Create a form that submits input when using the return key

We want to be able to create a new note just by typing and hitting return.  Let's create a component specifically intended for that.

``` /imports/components/forms/single_field_submit.jsx ```

```js
import React from 'react'

export class SingleFieldSubmit extends React.Component {

  constructor(props){
    super(props)
    this.state = {
      inputValue: this.props.inputValue
    }
  }

  updateInputValue(e){
    this.setState({inputValue: e.target.value})
  }

  handleSubmit(e) {
    e.preventDefault()
    this.props.handleSubmit(this.state.inputValue)
    this.setState({inputValue: ""})
  }

  render() {
      return <form  className="form-inline" onSubmit={this.handleSubmit.bind(this)}>
        <input
          type="text"
          className="form-control"
          placeholder={this.props.placeholder}
          value={this.state.inputValue}
          onChange={this.updateInputValue.bind(this)}
        />
      </form>
  }
}
SingleFieldSubmit.propTypes = {
  handleSubmit: React.PropTypes.func.isRequired,
  placeholder: React.PropTypes.string
}

SingleFieldSubmit.defaultProps = {
  inputValue:  ""  ,
  placeholder: "New..."
}
```

TODO: Be sure to discuss usage of this and bind(this)
-  Why are we using React.Component in this case?
-  What's the purpose of the constructor?
-  Why are we naming our component in this way?


## Add the form to the homepage

``` /imports/components/pages/homepage.jsx ```

```js
...
import { SingleFieldSubmit } from '../forms/single_field_submit'

export const Homepage = (props) => {

  ...
            <div id="main-content">
             <SingleFieldSubmit
               handleSubmit={props.handleCreateNote}
               placeholder={"New Note..."}
             />
            </div>
 ....

```

Now, if you try entering some text, you'll notice that it just disappears after you hit return.  Let's make sure we actually inserted some data...

## Install a utility for viewing db info in the browser

Rather than have to keep opening a Mongo console, let's install a utlity for doing this directly in the browser.  [Mongol](https://github.com/msavin/Mongol) is great for this.

Simply type ``` meteor add msavin:mongol ```
After that, you can use Ctrl + m to display an in-browser console.

![mongol-console](https://cloud.githubusercontent.com/assets/819213/15807700/e535c3c2-2b32-11e6-85d0-01c9890788cb.png)









