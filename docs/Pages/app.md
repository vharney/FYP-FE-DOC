---
sidebar_position: 1
---

# App.js

This pages is the first to be loaded when you first run <code>npm start</code> in the terminal. Hence, this page is the file where we will put our Routing behaviour in. As you can see from the import this pages basically import the **Routing library** that we use in this project and also import all other pages and assign them to a specific route for later navigation use. 

The routing library that we use is react-navigation. You can take a look at their website **[here](https://reactnavigation.org/)**

```jsx title="App.js import"
import React from 'react';
import { NavigationContainer } from '@react-navigation/native';
import { createNativeStackNavigator } from '@react-navigation/native-stack';
import { SafeAreaProvider } from 'react-native-safe-area-context';
import { View } from 'react-native';
import { useFonts } from 'expo-font';

import Main from './screens/Main';
import Profile from './screens/User/Profile';
import Login from './screens/Login';
import Signup from './screens/Signup';
import EditPersonalDetails from './screens/User/user-pages/EditPersonalDetails';
import EditHealthDetails from './screens/User/user-pages/EditHealthDetails';
import ManageWearable from './screens/Wearable/ManageWearable';
import ConfirmAuth from './screens/Wearable/ConfirmAuth';
import SuccessSplash from './screens/Wearable/SuccessSplash';
import SelfInputForm from './screens/SelfInputForm';
import AccountCreateSplash from './screens/AccountCreateSplash';
import Group from './screens/Community/Group';
import Article from './screens/Community/Article';
import AllRisk from './screens/Risk/AllRisk';
import ChangePassword from './screens/User/ChangePassword';
```
## Routing Logic

The important part to understand of this file is to first initially create a stack navigator object which will contain all the component for the routing, including the Navigator object and the child of the object which will be screens and the assigned componend and routes for the specific screen. 
```jsx
const Stack = createNativeStackNavigator();
```

Then, we can just assign the routes and screen based on the import above, example shown below.
```jsx
<NavigationContainer> 
    <Stack.Navigator screenOptions={{ animation: 'none'}} >
        <Stack.Screen name="Login" component={Login} options={{ headerShown: false }} />
        <Stack.Screen name="Signup" component={Signup} options={{ headerShown: false }} />
        <Stack.Screen name="Main" component={Main} options={{ 
            header: () =>
            (<View style={{ height: 35 }}/>)
        }}/>
        ...
    </Stack.Navigator>
</NavigationContainer>
```

:::tip Tip:
An important thing to note is the placement of the first screen in the navigator, whichever screen is placed first will be the first to load when the app is opened. In this case, the <code>Login</code> screen.
:::

Here, we set the animation of switching pages to be none as can be seen from the <code>screenOptions</code> paramater of <code>Stack.Navigator</code> as we found them to be quite disturbing for the user. 

As for the <code>Stack.Screen</code> component, the <code>name</code> parameter will specify the name of the pages that will be displayed on the screen in the header title of the screen. the <code>component</code> parameter will be the component or screens that will be rendered based on the import. Lastly, the <code>options</code> parameter has several options and modifications that we can change based on the default render for the specific screen. Here, two examples are we are disabling the header for the Login and Signup screen. Wherease for the Main screen, we are creating custom component for the header of the screen.

More detail on the parameters and other options for further customization can be found on the website **[here](https://reactnavigation.org/)**.
