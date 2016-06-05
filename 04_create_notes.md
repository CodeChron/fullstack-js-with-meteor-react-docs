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

If you remove the  ``` handleSubmit={props.handleSubmit}  ``` portion, the component displays on the homepage, but if you try typing in something, you'll notice nothing happens and we get some warnings and errors in our client console.  Let's fix that...







