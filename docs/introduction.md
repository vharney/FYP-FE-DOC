---
sidebar_position: 1
---

# Introduction

This is a documentation for the front-end side of the application. 

Please visit the GitHub link here to get the full source-code of the application **[here](https://github.com/memellisa/FYP-FE)**

Details of relevant <code>.js</code> file in the code will be explained in detail. To check out more detail on them click on the specific pages you want to know more about on the navbar on the left-hand-side of the page.

## Development Tools Used

The app is mainly developed with **React-Native expo app**, and npm libraries which will be listed below.

Expo is an open-source platform for making universal native apps for Android, iOS, and the web with JavaScript and React.
Check out **[Expo](https://expo.dev/)** for more information.

### UI Library

The main UI library used for the components of the mobile application are a combination of basic **[react-native](https://reactnative.dev/)** library and **[rneui](https://reactnativeelements.com/)** ui library, which stands for React Native Elements. 

With these libraries, we can develop our app more efficiently since we can use a pre-defined components inside the libraries instead of making them from scratches.

### Other libraries

Here are the list of other libraries and version that is used throughout the project:
1. "@react-native-async-storage/async-storage": "~1.17.3",
2. "@react-native-firebase/app": "~15.4.0",
3. "@react-navigation/bottom-tabs": "^6.4.0",
4. "@react-navigation/native": "^6.0.13",
5. "@react-navigation/native-stack": "^6.9.1",
6. "@react-navigation/stack": "^6.3.3",
7. "@rneui/base": "^4.0.0-rc.7",
8. "@rneui/themed": "^4.0.0-rc.7",
9. "axios": "^1.3.3",
10. "date-fns": "^2.29.5"
11. "firebase": "^9.15.0",
12. "formik": "^2.2.9",
13. "graphql": "15.7.2",
14. "lodash": "^4.17.21",
15. "victory-native": "^36.6.8",
16. "yup": "^1.0.2"

You can also take a look at the <code>package.json</code> file inside the repository for a more detailed breakdown of libraries and dependancies of the project.

## How to Get Started

### Environment Setup
#### 1. Download Node.js
Go to [Node Website](https://nodejs.org/en/download/) and download the latest LTS version
then (in the file directory) run:
```
cd App
npm install -g create-react-native-app
npm install -g react-native-cli
```

### Running The App
#### 1. Download Expo App on your phone
Go to [Expo Website](https://expo.dev/client) or App Store (for iOS) or Google Play (for Android) and download the Expo App to run the application
#### 2. (Alternatively) Download Android Studio and XCode
Download Android Studio [here](https://developer.android.com/studio?gclid=Cj0KCQjwqoibBhDUARIsAH2OpWhiFZJRsuSpU4ZtypSuG0qP4YuG_Bpelr2yrSXCFFZvZD0g7M73Ie0aAkIDEALw_wcB&gclsrc=aw.ds) and XCode in the App Store (only for Mac users) for Android phone and iPhone simulators
#### 3. Run the app!
Run (in the file directory):
```
cd App
npm install
npm start
```
Then you will see a barcode that you can scan with your phone, and it will take you to your Expo App. You can also press `a` to open Android phone simulator on your computer, and `i` for iPhone simulator