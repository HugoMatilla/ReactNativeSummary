# REACT NATIVE
# 0 Init
`react-native init <project name>`

`react-native run-android`
Add adb to the env variables
```js

	export ANDROID_HOME=/Users/hugomatilla/Documents/AndroidSDKs/sdk
	export PATH=${PATH}:${ANDROID_HOME}/tools
	export PATH=${PATH}:${ANDROID_HOME}/platform-tools
````

`react-native run-ios`
[Open non https on iOS](http://blog.bigbinary.com/2016/07/27/open-non-https-sites-in-webview-in-react-native.html)


`Cmd-D` in the emulator and go to [Debugger URL](http://localhost:8081/debugger-ui)

# 1 Basics
## 1.1 index.ios/android.js
_index.ios.js_ or _index.android.js_
```js

	import React from 'react'
	import { Text, AppRegistry } from 'react-native'

	const App = () => { 
		return (
	  		<Text> Hello React </Text>
		)
	}

	AppRegistry.registerComponent('AppName', () => App)

	// or `const App = () => (<Text> Hello React </Text>)`
	
```

## 1.2 Function Component
* Present static data
* Can't handle fetching data
* Easy to write
_index.ios.js_

```js
	
	import Header from './src/components/Header'

	const App = () => (<Header />)
	
```

_header.js_

```js

	const Header = () => (<Text> Hello React </Text>)
```


## 1.3 Send Props from root to child
_index.ios.js_

```js
	
	import Header from './src/components/Header'

	const App = () => (<Header headerText={'Title'} />)	
```

_header.js_

```js

	const Header = (props) => (<Text> props.headerText </Text>)
```

## 1.4 Class Component
* Dynamic sources of data
* Handles data that changes in time
* Knows when and what to render

```js

	import React, {Component} from 'react'
	import {Text} from 'react-native'

	class MyComponent extends Component {
	  render () {
	    return (
	      <Text> Hello React </Text>
	    )
	  }
	}

	export default MyComponent

```

## 1.5 State
Init the state with the object you are going to use `state = {items:[]}`

Use only `this.setState` to update the content of the state.

```js
	
	state = {items:[]}

	componentWillMount () {
	    axios.get('http://starwars.wikia.com/api/v1/Articles/Top?expand=1')
	    .then(response => this.setState({items: response.data.items})
	    )
	  }

	  render() {
	  	return (this.state.items.map(item => <Text> item.title </Text>))
	  }
```
## 1.6 Passing children to components
_ItemDetail.js_
```js

	const ItemDetail = () => {
		return (
			<Card>
				<Text>Title</Text>
			</Card>
			)
	}

```

_Card.js_
```js

	const Card = (props) => {
	  return (
	    <View >
	      {props.children}
	    </View>
	  )
	}
```

In `{props.children}` Card will render what is the children of the calling component. In this case the caller is `ItemDetail` and the children `<Text>Title</Text>`

## 1.7 Buttons and Links

_caller_
```js

	<Button onPress={() => Linking.openURL('http://es.starwars.wikia.com/' + url)}>
      <Text> Action </Text>
    </Button>
```

_Button.js_
```js

	const Button = ({ onPress, children }) => {
	  return (
	    <TouchableOpacity onPress={onPress}>
	      <View>
	        {children}
	      </View>
	    </TouchableOpacity>
	  )
	}
```

The `onPress` action passed in the `caller` will be performed by the button. 

The `children` will be rendered in the Button.

`TouchableOpacity` is one of the types of system buttons

# 2 Intermediate
## 2.1 Common components
* Add all the components to the same folder ie. `common`
* Add a `index.js` file to this folder.
* In `index.js` import and export the components.

```js
		export * from './Header'
		export * from './Button'
		...
```

* Export each component without `default` keyword

	` export  { Header : Header } ` or directly if the key and value are the same `export  { Header } `


* In the calling component import directly from the folder 

```js
 import { Header } from './common'
```

## 2.2 TextIputs and state
We tell using state what the value is.

```js

	<TextInput 
        value={this.state.text}
        onChangeText={text => this.setState({text})}
      />
```

## 2.3 Toggle views and binding
Bind the context (this) to the function: `onPress={this.onButtonPress.bind(this)}`   
[Binding Docs](https://facebook.github.io/react/docs/handling-events.html)   
[Binding Approaches](https://medium.com/@housecor/react-binding-patterns-5-approaches-for-handling-this-92c651b5af56#.a2wnywfmj)   

Using helper functions and returning a JSX 
```js

	renderButton() {
	    if (this.state.loading) {
	     	 return <Spinner size="small" />;
	    } else {
		    return (
		      <Button onPress={this.onButtonPress.bind(this)}>
		        Log in
		      </Button>
		    );
		}
	}
	
	...

	render(
		...
		<CardSection>
	  		{this.renderButton()}
		</CardSection>
	)
```
More bindings
```js
   
	firebase.auth().signInWithEmailAndPassword(email, password)
	  .then(this.onLoginSuccess.bind(this))
	  .catch(() => {
	    firebase.auth().createUserWithEmailAndPassword(email, password)
	      .then(this.onLoginSuccess.bind(this))
	      .catch(this.onLoginFail.bind(this));
	  });
	}
```
# 3 Redux

* **Action**:	An object that tells the reducer how to change its data
* **Store**: An object that holds the applications data	(Reducer & State)
* **Reducer**: A function that returns some data
* **State**: Data for our app to use.	


#### 1. The `Store` needs a reducer

```js

	const store = Redux.createStore(reducer)
```	

#### 2. A `Reducer` is a function (arrow). 
This one returns an array

```js

	const reducer = () => []
```

#### 3. The `Store` has the state

```js

	store.getState() 
	>[] // the result of the reducer function
```

If we change the `Reducer` output the `Store` state changes

```js

	const reducer = () => [123]

	store.getState() 
	>[123] 

```

#### 4. The `Action`  is a plain JS Object that tells the `Reducer` that it needs to modify the state it is producing.

The `Action` **MUST** define a `type` property that is a _string_.    

`payload` is the data we want to use in the `Action`

```js

	const action= {
	  type: 'splitString',
	  payload: 'asdf'
	}

```

#### 5. The `Reducer` handles the action

```js

	const reducer = (state = [], action) => {
		if (action.type === 'splitString'){
	   		 return action.payload.split('')
	 	}
	  
	  return state
	}
```

#### 6. And the `Store` send the `Action` to the `Reducer`

```js

	store.dispatch(action)
```

#### 7. A final `getState` is needed to run the Action

```js
	
	store.getState() 
```

#### 8. Add a new action

```js

	const reducer = (state = [], action) => {
		if (action.type === ...){
		   ...
		}
		else if (action.type === 'addChar'){
			state.push(action.payload)
		    return state
		}
	  
	  	return state
	}
```

#### 9. **NEVER MODIFY THE STATE OBJECT: CREATE A NEW ONE**
**BAD**
```js

	state.push(action.payload)
```
**RIGHT**
ES6 Syntax to create a new object from another
```js

	return [...state, action.payload]
```
## Boilerplate
2 libraries needed

* `> npm install --save redux` 
* `> npm install --save react-redux`

### General
![][image1]

[image1]: ./img/redux1.png 

### Provider
`Provider` holds `Stores` of our application, and allow the application to communicate with them.    
`Provider` communication between `React` and `Redux`    
`Provider` Can have only one single child

_app.js_

```js

	import React, {createStore} from 'react'
	import {View} from 'react-native'
	import {Provider} from 'react-redux'
	import reducers from './reducers'

	const App = () => {
	  return (
	    <Provider store={createStore(reducers)}>
	      <View />
	    </Provider>
	  )
	}

	export default App

	
```

### Reducers

`combineReducers` allows easily work with several `Reducers`

_src/reducers/index.js_

```js

	import {combineReducers} from 'redux'
	import LibraryReducer from './LibraryReducer'

	export default combineReducers({
	  libraries: LibraryReducer
	})
```

_src/reducers/LibraryReducer.js_
```js
	
	import data from './LibraryList.json'

	export default () => data
```	
 
### Connect State to Components
Use the `connect` helper.

`export default connect(<"What you want the Props to be">)("The component to add the Props")`

`import { connect } from 'react-redux'`

```js

	export default connect(mapStateToProps)(LibraryList)

```

To send the state to the component we need to send is as `Props`
```js

	const mapStateToProps = state => {
		return { libraries: state.libraries }
	}
```

_allTogether_
```js

	class LibraryList extends Component {
	  render () {
	  	console.log(this.props)
	    return ;
	    
	  }
	}

	const mapStateToProps = state => {
	  return { libraries: state.libraries }
	}

	export default connect(mapStateToProps)(LibraryList)

	> libraries: [....]
```

## Action Creator
`Action Creator`

```js

	() => {
	  return {
	    type: 'actionName'
	  }
```

_src/actions/index.js_
```js

	export const selectLibrary = (libraryId) => {
	  return {
	    type: 'select_library',
	    payload: libraryId
	  }
	}
```

### Calling `Action Creators`

`connect` second argument is used to bind/send `Action Creators` to the component as `props`.

`export default connect(null, actions)(ListItem)`

After that the component can call them to create `Actions` (that will be performed by the `Reducer` and will change the `State` of the `Store`)  


```js

	import * as actions from '../actions' // get all actions


	export default connect(null, actions)(ListItem)
```

### Reducers 2
`state = null` returns null in case state is `undefined`
```js

	export default (state = null, action) => {
	  switch (action.type) {
	    case 'select_library':
	      return action.payload
	    default:
	      return state
	  }
	}

```

## Moving logic out of components
Add logic to the `mapStateToProps`

Use `ownProps` as second argument to add new `props`

```js

	const mapStateToProps = (state, ownProps) => {
	  const expanded = state.selectedLibraryId === ownProps.library.id

	  return { expanded }
	}

	export default connect(mapStateToProps, actions)(ListItem)
```
## Redux Constraints
* Single State Tree (Plain Object)
* Actions Describe Updates (Plain Object)
* Reducers Apply Updates (Pure functions. Receive a state and an action and returns a new state. No more data is needed to perform it)

# 4 UI 
## ListView

```js

	import { ListView } from 'react-native' 

	class MyComponent extends Component {
	  constructor() {
	    super();
	    const ds = new ListView.DataSource({rowHasChanged: (r1, r2) => r1 !== r2});
	    this.state = {
	      dataSource: ds.cloneWithRows(['row 1', 'row 2']),
	    };
	  }

	  render() {
	    return (
	      <ListView
	        dataSource={this.state.dataSource}
	        renderRow={(rowData) => <Text>{rowData}</Text>}
	      />
	    );
	  }
	}


```

## Animation
```js

	import {LayoutAnimation} from 'react-native'

	class ListItem extends Component {
	  componentWillUpdate () {
    	LayoutAnimation.spring()
  	}


```	

# Complex Redux
## Async Action Creators - `Redux Thunk`
| Standard                                          | Thunk                                       |
|---------------------------------------------------|----------------------------------------------|
| `Action Creators` are functions                   | `Action Creators` are functions              |
| Must return an `Action`                           | Must return a **function**                   |
| An `Action` is an object with a `type` property   | This function will be called with `dispatch` |

`npm install --save redux-thunk`

![][image2]

[image2]: ./img/redux2.png 

```js

	import { createStore, applyMiddleware } from 'redux'
	import ReduxThunk from 'redux-thunk'

	...
    const store = createStore(reducers, {}, applyMiddleware(ReduxThunk));

    return (
      <Provider store={store}>
      ...
```

_actions.js_
```js

	export const loginUser = ({ email, password }) => {
	  return (dispatch) => {
	    firebase.auth().signInWithEmailAndPassword(email, password)
	      .then(user => {
	      	dispatch({ type: LOGIN_USER, payload: user })
	      	})
	      }
	    } 
	}
```

# Appendix
## Errors
#### Error: Element type is invalid: expected a string (for built-in components)
```js

	import { LibraryList } from './components/LibraryList'; 
```

We don't use the curly braces when we export an item with the default keyword.  The library list is exported from 'LibraryList.js' with the 'default' keyword, so remove the curly braces here.
