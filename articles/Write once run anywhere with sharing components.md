## Write once, run anywhere

Ever wonder writing only one piece of code but run it across all platforms (web + native)? Yes, you definitely can!

## Start from Native with [react-ative](https://facebook.github.io/react-native/)

* Initiate a react native project with [react-native-cli](https://www.npmjs.com/package/react-native-cli)

```sh
react-native rnweb
```

A project with the following directory structure will be created:

![Directory](https://user-images.githubusercontent.com/7504237/59961989-f4780600-9511-11e9-8462-a74979e3476b.jpg)

```js
// App.js code
import React, {Component} from 'react';
import {Platform, StyleSheet, Text, View} from 'react-native';

const instructions = Platform.select({
  ios: 'Press Cmd+R to reload,\n' + 'Cmd+D or shake for dev menu',
  android:
    'Double tap R on your keyboard to reload,\n' +
    'Shake or press menu button for dev menu',
});

type Props = {};
export default class App extends Component<Props> {
  render() {
    return (
      <View style={styles.container}>
        <Text style={styles.welcome}>Welcome to React Native!</Text>
        <Text style={styles.instructions}>To get started, edit App.js</Text>
        <Text style={styles.instructions}>{instructions}</Text>
      </View>
    );
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    backgroundColor: '#F5FCFF',
  },
  welcome: {
    fontSize: 20,
    textAlign: 'center',
    margin: 10,
  },
  instructions: {
    textAlign: 'center',
    color: '#333333',
    marginBottom: 5,
  },
});
```

```js
// index.js code
import {AppRegistry} from 'react-native';
import App from './App';
import {name as appName} from './app.json';

AppRegistry.registerComponent(appName, () => App);
```

> You can checkout the source code at **react-native** branch of the [repo](https://github.com/n0ruSh/rnweb)

* Run IOS

```sh
react-native run-ios
```

![Ios](https://user-images.githubusercontent.com/7504237/59961992-09ed3000-9512-11e9-8218-8c7baf4a0b7f.png)

* Run Android

```
react-native run-android
```
![Android](https://user-images.githubusercontent.com/7504237/59962001-1d989680-9512-11e9-9756-9989b673027f.png)

## Make it work for Web

Ideally we would like our react native project to work on web as well, with minimum code modification. The goal can be achieved with the help of [react-native-web](https://github.com/necolas/react-native-web).

### Principle

In the react native code, we import modules like **AppRegistry** and **Text** from [react-native](https://facebook.github.io/react-native/) package. These modules aren't recognized by web browsers and thus won't work if used directly on web platform. [react-native-web](https://github.com/necolas/react-native-web) implements the same protocols/APIs in web terminology used in [react-native](https://facebook.github.io/react-native/). By aliasing **react-native** to **react-native-web** in webpack config, we are then able to use the equivalent web components without code change. 

```js
//webpack.config.js
resolve: {
  extensions: [".js", ".jsx"],
  alias: {
    "react-native": "react-native-web"
  }
},
```

Packaging with the above webpack config with the following code

```
import {Text} from 'react-native'
```

=> actually imports **Text** component from 'react-native-web'.

Let's take a look into the [source code of **Text** component implementation](https://github.com/necolas/react-native-web/blob/master/packages/react-native-web/src/exports/Text/index.js) in [react-native-web](https://github.com/necolas/react-native-web). In the **render** function the return is either a **div** or **span** element which works on web platform.

```js
// Text/index.js
import applyLayout from '../../modules/applyLayout';
import applyNativeMethods from '../../modules/applyNativeMethods';
import { bool } from 'prop-types';
import { Component } from 'react';
import createElement from '../createElement';
import StyleSheet from '../StyleSheet';
import TextPropTypes from './TextPropTypes';

class Text extends Component<*> {
  //...

  render() {
    const {
      dir,
      numberOfLines,
      onPress,
      selectable,
      style,
      /* eslint-disable */
      adjustsFontSizeToFit,
      allowFontScaling,
      ellipsizeMode,
      lineBreakMode,
      minimumFontScale,
      onLayout,
      onLongPress,
      pressRetentionOffset,
      selectionColor,
      suppressHighlighting,
      textBreakStrategy,
      tvParallaxProperties,
      /* eslint-enable */
      ...otherProps
    } = this.props;

    //...

    const component = isInAParentText ? 'span' : 'div';

    return createElement(component, otherProps);
  }

  // ...
}
// ...

export default applyLayout(applyNativeMethods(Text));
```

> react native bundling doesn't go through webpack while web packaging does. Thus the aliasing in webpack config only works for web packaging.

### Steps Details

* Add **web** folder in parellel with **android** and **ios**
* Add **index.html** under **web** folder

```html
<!DOCTYPE html>
<html>
  <head>
    <title>React Native Web</title>
    <meta name="viewport" content="width=device-width">
  </head>
  <body>
    <div id="root"></div>
  </body>
</html>
```
* Install dependency

```sh
npm install --save react-art react-dom react-native-web 
html-webpack-plugin webpack-dev-server webpack webpack-cli 
babel-loader @babel/preset-env @babel/preset-react
```

* Add webpack config file: **webpack.config.js**

```js
const HtmlWebPackPlugin = require("html-webpack-plugin");

module.exports = {
  entry: {
    app: "./index.js"
  },
  resolve: {
    extensions: [".js", ".jsx"],
    alias: {
      "react-native": "react-native-web"
    }
  },
  output: {
    filename: "bundle.js"
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /(node_modules|bower_components)/,
        use: {
          loader: "babel-loader",
          options: {
            presets: ["@babel/preset-env", "@babel/preset-react"]
          }
        }
      }
    ]
  },
  plugins: [
    new HtmlWebPackPlugin({
      template: "./web/index.html",
      filename: "./index.html"
    })
  ],
  devServer: {
    historyApiFallback: true
  }
};
```

* Add quick start command in **scripts** attribute in package.json

```js
{
  web: "webpack-dev-server"
}
```

* Update **App.js**

```js
// App.js
import React, {Component} from 'react';
import {Platform, StyleSheet, Text, View} from 'react-native';

const instructions = Platform.select({
  ios: 'Press Cmd+R to reload,\n' + 'Cmd+D or shake for dev menu',
  android:
    'Double tap R on your keyboard to reload,\n' +
    'Shake or press menu button for dev menu',
  web: 'Cmd+R or F12 to reload'
});

type Props = {};
export default class App extends Component<Props> {
  render() {
    return (
      <View style={styles.container}>
        <Text style={styles.welcome}>Welcome to React Native!</Text>
        <Text style={styles.instructions}>To get started, edit App.js</Text>
        <Text style={styles.instructions}>{instructions}</Text>
      </View>
    );
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    backgroundColor: '#F5FCFF',
  },
  welcome: {
    fontSize: 20,
    textAlign: 'center',
    margin: 10,
  },
  instructions: {
    textAlign: 'center',
    color: '#333333',
    marginBottom: 5,
  },
});
```

* Update **index.js**

```js
//index.js
import {AppRegistry, Platform } from 'react-native';
import App from './App';
import {name as appName} from './app.json';

AppRegistry.registerComponent(appName, () => App);
if (Platform.OS === 'web') {
  AppRegistry.runApplication(appName, {
    rootTag: document.getElementById("root")
  });
}
```

* Run

```sh
npm run web
```
![Web](https://user-images.githubusercontent.com/7504237/59962012-44ef6380-9512-11e9-8d44-723569264d8d.png)

## Reference

* [react-native](https://facebook.github.io/react-native/)
* [react-native-web](https://github.com/necolas/react-native-web)
* [Example Source Code](https://github.com/n0ruSh/rnweb)

## Notice

* If you want to follow the latest news/articles for the series of my blogs, Please [「Watch」](https://github.com/n0ruSh/blogs/)to Subscribe.