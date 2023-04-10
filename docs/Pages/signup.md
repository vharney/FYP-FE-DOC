---
sidebar_position: 3
---

# Signup.js

The signup screen has similar UI and logic to the Login.js screen. The code also uses **[Formik](https://formik.org/)** library to handle form validation and submission for the signup information. Then, once the user inputted the credentials and login (submit the form). The code uses functions from the **[Firebase](https://rnfirebase.io/auth/usage)** library to handle the auth status and manages session.  

## Formik

```jsx
<Formik
    validateOnMount={true}
    validationSchema={signupValidationSchema}
    initialValues={{ email: '', password: '', confirmPassword: '' }}
    onSubmit={values => onPress(values)}
    >
    {({ handleChange,
        handleBlur,
        handleSubmit,
        values,
        errors,
        touched,
        isValid, }) => {
        return <>
        <View style={styles.inputView} >
            <Input value={values.email} 
                onChangeText={handleChange('email')}  
                onBlur={handleBlur('email')} 
                keyboardType="email-address" 
                placeholder='Email' 
                style={styles.input} 
                inputContainerStyle={{borderBottomWidth: 0}}
                errorMessage={(errors.email && touched.email) ? errors.email : ''}
            />
            <Input onChangeText={handleChange('password')}
                onBlur={handleBlur('password')}
                value={values.password} 
                placeholder='Password' 
                style={styles.input} 
                secureTextEntry={true} 
                inputContainerStyle={{borderBottomWidth: 0}}
                errorMessage={(errors.password && touched.password) ? errors.password : ''}
            />
            <Input onChangeText={handleChange('confirmPassword')}
                onBlur={handleBlur('confirmPassword')}
                value={values.confirmPassword} 
                placeholder='Confirm Password' 
                style={styles.input} 
                secureTextEntry={true} 
                inputContainerStyle={{borderBottomWidth: 0}}
                errorMessage={(errors.confirmPassword && touched.confirmPassword) ? errors.confirmPassword : ''}
            />
        </View>
        
        <Button title="Signup" buttonStyle={styles.button} 
            onPress={handleSubmit}
            disabled={!isValid || values.email === '' || values.password === '' || values.confirmPassword === ''}
        />
        <Button 
            title="Have an account? Login instead!" 
            buttonStyle={styles.button} 
            onPress={() => navigation.navigate('Login')}
        />
        </>
        }}
</Formik>
```

This component works in a similar fashion to the `<Formik>` component in the `Login.js` file. Once you understand that, understanding this code should not be difficult.

The only difference here is the addition of another `<Input>` component to confirm that user has inputted the correct password as they want.

### Form Validation

The schema of the validation is then modified accordingly.

```jsx
const signupValidationSchema = yup.object().shape({
    email: yup
        .string()
        .email("Please enter valid email")
        .required('Email Address is required'),
    password: yup
        .string()
        .min(6, ({ min }) => `Password must be at least ${min} characters`)
        .required('Password is required'),
    confirmPassword: yup
        .string()
        .oneOf([yup.ref('password'), null], 'Passwords must match')
})
```

The key difference to the validation schema here to the `Login.js` page is the addition of `oneOf` function which refers to the password variable and ensure they must match. 

## Sign Up Firebase Authentication

Once users has passed the validation, users can then submit and try to create their account. Similarly, using the `onSubmit` parameters, we have set a custom defined function to help handle this submit event. 

```jsx
const onPress = async (values) => {
    if (values.email === '' || values.password === '' || values.confirmPassword === '') {
        Alert.alert('Email or password is empty')
        return
    }
    let payload = JSON.stringify({ 'email': values.email, 'password': values.password })
    try {
        const response = await axios.post(`${flaskURL}/auth/signup`, payload,{
            headers: {
                'Content-Type': 'application/json'
            }
        });

        if (response.data == "Account successfully created") {
            signInWithEmailAndPassword(auth, values.email, values.password)
                .then((userCredential) => {
                    // Signed in and will trigger onAuthStateChanged
                    navigation.push("Create Account Splash")
                    setTimeout(() => {
                    // should show the Connected Manage Wearable page instead later
                    // navigation.navigate("Profile");
                    navigation.navigate("Self Input Form", {data: values})
                    }, 3000);
                })
                .catch((error) => {
                    // Log the error
                    const errorCode = error.code;
                    const errorMessage = error.message;
                    if (errorCode === 'auth/invalid-email' || errorCode === 'auth/user-not-found' || errorCode === 'auth/wrong-password') {
                        Alert.alert('Email or password is incorrect')
                    } else {
                        Alert.alert('Something went wrong during sign in, please sign in again')
                        navigation.replace("Login")
                    }
                });
        }
        else {
            // console.log(response)
            alert("Error creating account \n Reason: " + response.data)
        }
    } catch (error) {
        alert("Error creating account \n Reason: " + error)
    } 
}
```

The key difference here is the additional `axios` request to our `Flask` backend server. The reason we did this is to create a document in our firebase storage to help store the information of user, which will be explained more in `SelfInputForm.js` documentation page. 

If the response from our `Flask` server is as expected, we will then proceed to sign in the user again similar to the logic in `Login.js` and push a `CreateAccountSplash.js` screen as feedback that account is successfully created. If there is no user data. Instead of navigation to `Main.js` directly, we ask the user to fill in the required data first in `SelfInputForm.js` before moving directly to the Main screen. 

The users will be given an Alert in case any error occured.

## Button to Login Screen

The last component is the `<Button>` to go back to the Login screen immediately.