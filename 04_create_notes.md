# Form for creating notes

A good approach to implementing a feature is often to "follow the data."  Here, we'll do so first creating a form for creating a note and then adding a handler for inserting that note into our database.

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

export const Homepage = () => {

  ...
            <div id="main-content" className="container">
              <SingleFieldSubmit
                placeholder={"New Note..."}
                handleSubmit={props.handleSubmit}
              />
            </div>
 ....

```

If we remove the ```handleSubmit ```, the component displays on the homepage, but if you try typing in something, you'll notice nothing happens and we get some warnings and errors in our client console.



## Notes - move to later section
Here we will create a single "Controller" component for managing:
- Creating a note
- Listing existing notes

Add a NotesContainer
**Render NotesContainer as a top-level component (this is new from the preview version, we need to do this because we want to have a top-level container structure to handle component state for the entire page)**

(when we add users, we'll switch this to users container)







