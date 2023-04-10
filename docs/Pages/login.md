---
sidebar_position: 2
---

# Login.js

The login screen is the first screen that will be loaded when opening the app for the first time from a terminated state. The code uses **[Formik](https://formik.org/)** library to handle form validation and submission for the login information. Then, once the user inputted the credentials and login (submit the form). The code uses functions from the **[Firebase](https://rnfirebase.io/auth/usage)** library to handle the auth status and manages session.  

## Formik

```jsx
<Formik
    validateOnMount={true}
    validationSchema={loginValidationSchema}
    initialValues={{ email: '', password: '' }}
    onSubmit={values => login(values)}
  >
    {({ handleChange,
      handleBlur,
      handleSubmit,
      values,
      errors,
      touched,
      isValid, }) => (
      <>
        <View style={styles.inputView} >

            <Input 
              value={values.email} 
              onChangeText={handleChange('email')}  
              onBlur={handleBlur('email')} 
              keyboardType="email-address" 
              placeholder='Email' 
              style={styles.input} 
              inputContainerStyle={{borderBottomWidth: 0}}
              errorMessage={(errors.email && touched.email) ? errors.email : ''}
            />
            <Input 
              onChangeText={handleChange('password')}
              onBlur={handleBlur('password')}
              value={values.password} 
              placeholder='Password' 
              style={styles.input} 
              secureTextEntry={true} 
              inputContainerStyle={{borderBottomWidth: 0}}
              errorMessage={(errors.password && touched.password) ? errors.password : ''}
            />
            
        </View>
    
        <Button title="Login" buttonStyle={styles.button} 
            loading={loading}
            onPress={handleSubmit}
            disabled={!isValid || values.email === ''}
        />
        <Button 
            title="Don't have an account? Signup today!" 
            buttonStyle={styles.button} 
            onPress={() => {
              navigation.navigate('Signup')
            }}
        />

        
      </>
    )}
  </Formik>
```

The Formik component helps us build a Form component with certain built in functions to help submit, validate and handle state changes for the components of the form. As can be seen, we have two initial input variables (email and password), each assigned to an `<Input>` component. Each `<Input>` component then have their own handler when user input text into the field, i.e. `onChange`, or when user end editing, i.e. `onBlur`. 

To help handle the validation, we can set a `validationSchema` parameter for the Formik component and assign the required input for each value.

### Form Validation

The library `yup` which have been integrated into the Formik component is used to help us build the validation schema. More details of the library can be checked **[here](https://formik.org/docs/guides/validation)**.

The schema of the validation is created inside the `validation.js` file located inside the utils folder.

```jsx
import { loginValidationSchema } from '../utils/validation';
```

Here is the schema used for login,

```jsx
import * as yup from 'yup'

const loginValidationSchema = yup.object().shape({
    email: yup
      .string()
      .email("Please enter valid email")
      .required('Email Address is required'),
    password: yup
      .string()
      .min(6, ({ min }) => `Password must be at least ${min} characters`)
      .required('Password is required'),
})

export { loginValidationSchema, ... }
```

As can be seen, we need to first create a `yup` object and assign the requirement for each of the value we have initialized in the Formik component. 

In the case of login:
1. For `email`, we need it to be a string, a valid email address string, and the field cannot be empty
2. For `password`, we need it to be a string, a minimum of 6 characters, and the field cannot be empty

Check out the yup documentation for more information on other validation functions that can be used.

## Firebase Authentication

Lastly, when user finish they can submit the Form using the `onSubmit` parameter defined in Formik component. The way we have done this is by creating a button and assign the `handleSubmit` handler into the `onClick` behaviour of the button. Which in turn, will call a custom login function we have defined as follow.

```jsx
const login = (values) => {
    setLoading(true)
    if (values.email === '' || values.password === '') {
        Alert.alert('Email or password is required')
    }
    else {
        signInWithEmailAndPassword(auth, values.email, values.password)
        .then((userCredential) => {
            // Signed in and will trigger onAuthStateChanged
        })
        .catch((error) => {
            const errorCode = error.code;
            const errorMessage = error.message;
            if (errorCode === 'auth/invalid-email' || errorCode === 'auth/user-not-found' || errorCode === 'auth/wrong-password') {
                Alert.alert('Email or password is incorrect')
            } else {
                Alert.alert('Something went wrong')
            }
        });
        setLoading(false);
    }
}
```

The function takes in all the values of the Formik component as a parameter. They can be accessed using the `.` operator according to the name of the variables set for each values. 

Then, we use `firebase` library to help check the credentials of the user, i.e. whether the user exist and the combination of email password is correct. If the credential is correct, the `.then` codeblock will run and change the `onAuthStateChanged` listener which have been defined globally to true. In case if any error occured. `.catch` block will run and Alert the user of the error returned by firebase, e.g. invalid email, no user found, and wrong password. 

The `onAuthStateChanged` is defined in the `useEffect` React hook to help subscribe the app whenever there is any Auth status changes in the application, e.g. login, logout. 

```jsx
useEffect(() => {
    setLoading(false)
    unsubscribe = onAuthStateChanged(auth, (user) => {
      if (user) {
        setLoading(true)
        // User is signed in, see docs for a list of available properties
        // https://firebase.google.com/docs/reference/js/firebase.User
        console.log("LOGGED IN")
        setTimeout(() => {
          // should show the Connected Manage Wearable page instead later
          // navigation.navigate("Profile");
          setLoading(false)
          navigation.navigate("Main");
        }, 2000);
        // ...
    } else {
        console.log("LOGGED OUT")
        navigation.navigate("Login")
      }
    });
}, []);
```

If the user is logged in, we will navigate directly to the `Main.js` page. Else, just redirect them to the `Login` page again. 

The parameter of `onAuthStateChanged` is `auth` which we have imported initially from the `config.js` file, linking our frontend application with the firebase backend containing the database and other auth functions. 

```jsx
import { auth } from '../config';

...

// Content of config.js
import { initializeApp } from 'firebase/app';
import { getAuth } from "firebase/auth";

// Initialize Firebase
const firebaseConfig = {
    apiKey: "AIzaSyD5wNTFbUHECtsxWrsiGyfwzSBCxAQwIk8",
    authDomain: "heart-f1ee1.firebaseapp.com",
    projectId: "heart-f1ee1",
    storageBucket: "heart-f1ee1.appspot.com",
    messagingSenderId: "1005939249005",
    appId: "1:1005939249005:web:ec968bc4617cb0e1375464",
    measurementId: "G-EMYYPP6VCL"
};

// Initialize Firebase
export const app = initializeApp(firebaseConfig);

// Initialize Firebase Authentication and get a reference to the service
export const auth = getAuth(app);
```

In the `config.js` file, we need to set the api key, project id and other required information accordingly. Read more on how to set firebase server backend **[here](https://firebase.google.com/docs/web/setup)**.


## Button to Signup Screen

The other relevant component that we have in the Login page is just a Button to redirect to the `Signup.js` page. This will be explained in more detail on the `Signup.js` documentation page. 