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

# 1
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

	const App = () => (<Header headerText'={ 'Title' } />)	
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
	      <Text>
	        {children}
	      </Text>
	    </TouchableOpacity>
	  )
	}
```

The `onPress` action passed in the `caller` will be performed by the button. 
The `children` will be rendered in the Button.

# 2
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

##2.3 Toggle views and binding
Bind the context(this) to the function: `onPress={this.onButtonPress.bind(this)}`
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
