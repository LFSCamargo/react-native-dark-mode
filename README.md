# react-native-dark-mode

[![npm version](https://img.shields.io/npm/v/react-native-dark-mode.svg)](https://www.npmjs.com/package/react-native-dark-mode)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](http://makeapullrequest.com)

<p align="center"><img src="https://raw.githubusercontent.com/codemotionapps/react-native-dark-mode/master/showcase.gif" alt="Showcase" width="200" height="433"></p>

## Installation

```sh
npm install react-native-dark-mode
react-native link react-native-dark-mode
```

## Usage

### High level APIs

#### `useDarkMode`

Returns a boolean. `true` when dark mode is on.

```javascript
import { useDarkMode } from 'react-native-dark-mode'

function Component() {
	const isDarkMode = useDarkMode()
	return <View style={{ backgroundColor: isDarkMode ? 'black' : 'white' }} />
}
```

#### `useDarkModeContext`

Returns `dark` or `light`.

```javascript
import { useDarkModeContext } from 'react-native-dark-mode'

const backgroundColors = {
	light: 'white',
	dark: 'black',
}

function Component() {
	const mode = useDarkModeContext()
	const backgroundColor = backgroundColors[mode]
	return <View style={{ backgroundColor }} />
}
```

#### `DynamicStyleSheet`, `DynamicValue` and `useDynamicStyleSheet`

Just like `StyleSheet` but with support for dynamic values.

```javascript
import { DynamicStyleSheet, DynamicValue, useDynamicStyleSheet } from 'react-native-dark-mode'

const dynamicStyles = new DynamicStyleSheet({
	container: {
		backgroundColor: new DynamicValue('white', 'black'),
		flex: 1
	},
	text: {
		color: new DynamicValue('black', 'white'),
		textAlign: 'center'
	}
})

function Component() {
	const styles = useDynamicStyleSheet(dynamicStyles)

	return (
		<View style={styles.container}>
			<Text style={styles.text}>My text</Text>
		</View>
	)
}
```

#### `DarkModeProvider`

Allows you to set a specific mode for children.

```javascript
import { DarkModeProvider } from 'react-native-dark-mode'

function MyScreen() {
	return (
		<>
			{/* will be rendered using dark theme */}
			<DarkModeProvider mode="dark">
				<Component />
			</DarkModeProvider>

			{/* will be rendered using light theme */}
			<DarkModeProvider mode="light">
				<Component />
			</DarkModeProvider>

			{/* will be rendered using current theme */}
			<Component />
		</>
	)
}
```

It is recommended to wrap your application in a `DarkModeProvider` without a `mode` prop to observe a performance improvement.

```javascript
function App() {
	return (
		<DarkModeProvider>
			{/* ... */}
		</DarkModeProvider>
	)
}
```

#### `useDynamicValue`

Returns the appropriate value depending on the theme. You can either pass a `DynamicValue` or just two arguments.

```javascript
import { DynamicValue, useDynamicValue } from 'react-native-dark-mode'
const lightLogo = require('./light.png')
const darkLogo = require('./dark.png')
const logoUri = new DynamicValue(lightLogo, darkLogo)

function Logo() {
	const source = useDynamicValue(logoUri)
	return <Image source={source} />
}
```

```javascript
import { useDynamicValue } from 'react-native-dark-mode'

function Input() {
	const placeholderColor = useDynamicValue('black', 'white')
	return <TextInput placeholderTextColor={placeholderColor} />
}
```

### Low level APIs

#### `initialMode`

This is the initial mode that the app started in.

```javascript
import { initialMode } from 'react-native-dark-mode'

console.log('App started in', intialMode, 'mode')
```

#### `eventEmitter`

Allows you to subscribe to changes in the mode.

```javascript
import { eventEmitter } from 'react-native-dark-mode'

eventEmitter.on('currentModeChanged', (newMode) => {
	console.log('Switched to', newMode, 'mode')
})
```

## Requirements

### iOS

- Xcode 11 beta 1 or higher
- React Native 0.59.9 or higher
- iOS 13 beta 1 or higher to see it in action
