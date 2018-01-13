# ReactNative-Starter-with-TypeScript
Guide to setup and run the first app on react native with typescript

One of the thing I have missed from React Native was support for typescript.
Coming from object oriented world typescript gives me better way to learn and develop apps using advanced javascript.

However, every single guide I saw for implementing TypeScript within React Native was flawed. 
Specifically, there was a need for a separate build step.

Well, I’ve solved that and this is how I did it.

Create an App

```
react-native init react-typescript
```

I am using React Native CLI . 
Setting up React native CLI can be found [here](https://facebook.github.io/react-native/docs/getting-started.html)

I have installed yarn before and will be using yarn.

Add TypeScript Packages

Go to the root folder of your project and run the comand from the terminal.

```
yarn add --dev react-native-typescript-transformer typescript @types/react @types/react-native
```



Configure TypeScript

TypeScript has a configuration file called tsconfig.json. create this file in the root folder of the project
with code,

```
{
    "compilerOptions": {
        "target": "es2015",
        "module": "es2015",
        "jsx": "react-native",
        "moduleResolution": "node",
        "allowSyntheticDefaultImports": true,
        "noImplicitAny": true,
        "strictNullChecks": true
    }
}
```

Configure the React Native Packager

This (along with the react-native-typescript-transformer package) is the bit that does the magic – compiling your TypeScript files on the fly!
Create a file called rn-cli.config.js in the root folder of the project with the following contents:

```
module.exports = {
    getTransformModulePath() {
        return require.resolve('react-native-typescript-transformer');
    },
    getSourceExts() {
        return [ 'ts', 'tsx' ]
    }
};
```

You will also want to update the start script definition within package.json:

```
"scripts": {
    "start": "react-native start --transformer node_modules/react-native-typescript-transformer/index.js --sourceExts ts,tsx",
    "test": "jest"
},
```

Write a TypeScript React Native component

I’ve created a folder called src that holds my TypeScript files. I’ve placed a component in src/index.tsx as follows:

```
import React from 'react';
import { StyleSheet, Text, TextStyle, View, ViewStyle } from 'react-native';
 
interface Props {
}
 
interface State {
}
 
const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    backgroundColor: '#F5FCFF',
  } as ViewStyle,
  welcome: {
    fontSize: 20,
    textAlign: 'center',
    margin: 10,
  } as TextStyle,
  instructions: {
    textAlign: 'center',
    color: '#333333',
    marginBottom: 5,
  } as TextStyle,
});
 
export default class App extends React.Component<Props, State> {
  render() {
    return (
      <View style={styles.container}>
        <Text style={styles.welcome}>
          Welcome to React Native!
        </Text>
        <Text style={styles.instructions}>
          {'To get started, edit src/index.tsx'}
        </Text>
      </View>
    );
  }
}
```

Change the import statement in index.js file in the root folder 

```
import { AppRegistry } from 'react-native';
import App from './src';

AppRegistry.registerComponent('react-typescript', () => App);
``

now start a terminal and add a typescript watcher to complie the file from .tsx to .js .

```
tsc -w
```

This command will complie the file and create a new file index.js in the src folder .

run your command 
```
react-native run-android

```

or 

```
react-native run-ios
```

thats all to start react native with typescript.

Happy Javascripting!

