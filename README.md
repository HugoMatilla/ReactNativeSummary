#REACT NATIVE
#0 Init
`react-native init <project name>`

`react-native run-android`
Add adb to the env variables
```

	export ANDROID_HOME=/Users/hugomatilla/Documents/AndroidSDKs/sdk
	export PATH=${PATH}:${ANDROID_HOME}/tools
	export PATH=${PATH}:${ANDROID_HOME}/platform-tools
````

`react-native run-ios`
[Open non https on iOS](http://blog.bigbinary.com/2016/07/27/open-non-https-sites-in-webview-in-react-native.html)

#1
##1.1 index.ios/android.js
_index.ios.js_ or _index.android.js_
```

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

##1.2 Function Component
* Present static data
* Can't handle fetching data
* Easy to write
_index.ios.js_
```
	
	import Header from './src/components/Header'

	const App = () => (<Header />)
	
```

_header.js_

```

	const Header = () => (<Text> Hello React </Text>)
	}
```


##1.3 Send Props from root to child
_index.ios.js_
```
	
	import Header from './src/components/Header'

	const App = () => (<Header headerText'={'Title'} />)
	
```

_header.js_

```

	const Header = (props) => (<Text> props.headerText </Text>)
	}
```

##1.4 Class Component
* Dynamic sources of data
* Handles data that changes in time
* Knows when and what to render

```

	class Header extends Component{
		render () {
			return <Text> Hello React </Text>
		}
	}
```

##1.5 State
Init the state with the object you are going to use `state = {items:[]}`
Use only `this.setState` to update the content of the state.

```
	
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
##1.6 Passing children to components
_ItemDetail.js_
```

	const ItemDetail = () => {
		return (
			<Card>
				<Text>Title</Text>
			</Card>
			)
	}

```

_Card.js_
```

	const Card = (props) => {
	  return (
	    <View >
	      {props.children}
	    </View>
	  )
	}
```

In `{props.children}` Card will render what is the children of the calling component. In this case the caller is `ItemDetail` and the children `<Text>Title</Text>`

##1.7 Buttons and Links

_caller_
```

	<Button onPress={() => Linking.openURL('http://es.starwars.wikia.com/' + url)}>
      <Text> Action </Text>
    </Button>
```

_Button.js_
```

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

#2
##2.1 Common components
* Add all the components to the same folder ie. `common`
* Add a `index.js` file to this folder.
* In `index.js` import and export the components.

```
		export * from './Header'
		export * from './Button'
		...
```

* Export each component without `default` keyword

	` export  { Header : Header } ` or directly if the key and value are the same `export  { Header } `


* In the calling component import directly from the folder 
``` import { Header } from './common'```



