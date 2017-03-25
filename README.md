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

#### 2. A `Reducer` is a function (arrow) that returns an array

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

#### 4. The `Action`  is a plain JS Object the tells the `Reducer` that it needs to modify the state it is producing.

The `Action` **must** define a `type` property that is a _string_.    

`payload` is the data we want to use in the 'Action'

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

### 8 Add a new action

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

**NEVER MODIFY THE STATE OBJECT: CREATE A NEW ONE**
**BAD**
```js
	
	state.push(action.payload)
```
**RIGHT**
ES6 Syntax to create a new object from another
```js

	return [...state, action.payload]
```