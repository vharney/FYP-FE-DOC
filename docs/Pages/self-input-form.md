---
sidebar_position: 4
---

# SelfInputForm.js

The self input form screen also uses **[Formik](https://formik.org/)** library to handle form validation and submission for the user data information. Then, once the user inputted the credentials and login (submit the form). The code will communicate with our `Flask` backend server to help fill out the `user` document in the firebase database according to ther `id` in the database with information that they have filled on the form. 

## Formik

There are some additional custom made components that we have designed to be integrated in the Formik component instead of just using basic components in our UI library. This is done to keep the code clean and help us build other components from other libraries such as date picker etc.

The information that we need from the user and will be the input of the form is the following:
1. firstName:'', 
2. lastName:'', 
3. dob:endDate, 
4. insulin:'',
5. cholesterol:'', 
6. diet:'', 
7. smokingStatus:'', 
8. alcoholConsumption:'', 
9. bloodPressure:'', 
10. sex:'', 
11. height:'', 
12. weight:'', 
13. bloodType:''

```jsx
<Formik
    validateOnMount={true}
    validationSchema={userInfoValidationSchema.concat(userHealthValidationSchema)}
    initialValues={{ 
        firstName:'', 
        lastName:'', 
        dob:endDate, 
        insulin:'',
        cholesterol:'', 
        diet:'', 
        smokingStatus:'', 
        alcoholConsumption:'', 
        bloodPressure:'', 
        sex:'', 
        height:'', 
        weight:'', 
        bloodType:''}}
    onSubmit={values => onPressDone(values)}
>
    {({ handleChange,
    handleBlur,
    handleSubmit,
    values,
    errors,
    touched,
    isValid, }) => (
    <>
        {InfoOverlay(infoVisible, toggleOverlay, infoMsg)}

        {InputTextField(
            'First Name', 
            values.firstName,
            handleChange('firstName'), 
            (errors.firstName && touched.firstName) ? errors.firstName : '', 
            handleBlur('firstName'))}

        {InputTextField(
            'Last Name', 
            values.lastName, 
            handleChange('lastName'), 
            (errors.lastName && touched.lastName) ? errors.lastName : '', 
            handleBlur('lastName'))}
        ...
        {DatePickerField('Date of Birth', openModal, handleOnPress, values.dob, handleChange('dob'))}
        ...
        {DropDownField(
            "Blood Type", 
            values.bloodType, 
            bloodData, 
            handleChange('bloodType'),  
            (val) => touched.bloodType = val, 
            (errors.bloodType && touched.bloodType) ? errors.bloodType : '')}
        ...
        <View style={styles.optionView}>
            <View style={styles.inputTitleView}>
                <Icon name="help" color="#0F52BA" size='18' onPress={() => onIconPress(formInfoMsgs.age)}/>
                <Text style={styles.fieldText}>Age</Text>
            </View>
            <Text style={styles.valueText}>{ !isNaN(values.dob) || !isNaN(calculateAge(values.dob)) ? calculateAge(values.dob) : '-'}</Text>
        </View>
        ...
        <View style={styles.optionView}>
            <View style={styles.inputTitleView}>
                <Icon name="help" color="#0F52BA" size='18' onPress={() => onIconPress(formInfoMsgs.bmi)}/>
                <Text style={styles.fieldText}>BMI</Text>
            </View>
            <Text style={styles.valueText}>{(values.height && values.weight && isFinite(countBMI(values.height, values.weight))) ? countBMI(values.height, values.weight) : '-'}</Text>
        </View>
        ...
    </>)}
</Formik>
```

### InputTextField Component

Most of the input form in the Self Input Form screen is made out of the `<InputTextField>` component which can be found in the `InputTextField.js` file under the `components` folder. The reason we made a custom component for this is to avoid cluttering our code in this file and repetitively writing the same code over and over.

The `<InputTextField>` component is actually just a form field designed to have a title for the input field on the left and a text field input box on the right. 

```jsx
const InputTextField = (text, value, onChangeText, errorMessage, handleBlur) => {
    const isNumber = text === 'Height (cm)' || text === 'Weight (kg)'
    return (
        <View>
            <View style={styles.inputView}>
                <View style={styles.inputTitleView}>
                    <Text style={styles.inputText}>{text}</Text>
                </View>
                <TextInput 
                    style={styles.valueText} 
                    value={value.toString()} 
                    onChangeText={onChangeText} 
                    multiline={isNumber ? false : true} 
                    keyboardType={isNumber ? 'decimal-pad' : 'default'}
                    onBlur={handleBlur}
                />
            </View>
            <Text style={styles.errorText}>{errorMessage}</Text>
        </View>
    
)}
```

As can be seen from the code, the custom component takes 5 parameters, 
1. text = the form title for that corresponding input
2. value = the current value for that input
3. onChangeText = the change text handler which will be passed from `Formik`
4. errorMessage = the error message passed dorm from `Formik` validation
5. handleBlur = the blur handler to set the state once user finish editing

### DatePickerField Component

For the date of birth of the user, we have used a custom component which is designed with the help of external library called `react-native-modern-datepicker`, more information can be found about the library **[here](https://www.npmjs.com/package/react-native-modern-datepicker)**. The `<DatePickerField>` component can be found in the `DatePickerField.js` file under the `components` folder.

```jsx
import { Modal, StyleSheet, Text, View, TouchableOpacity } from 'react-native';
import DatePicker from 'react-native-modern-datepicker'
import { getFormatedDate } from 'react-native-modern-datepicker'

const today = new Date()
const endDate = getFormatedDate(today.setDate(today.getDate()), 'YYYY/MM/DD')

const DatePickerField = (text, openModal, handleOnPress, date, handleChangeDate) => {
    return (
    <View style={styles.optionView}>
        <Text style={styles.optionText}>{text}</Text>
        <TouchableOpacity onPress={handleOnPress}>
        <Text style={styles.valueText}>{date}</Text>
        </TouchableOpacity>
        <Modal
            animationType='slide'
            transparent={true}
            visible={openModal}
        >
            <View style={styles.centeredView}>
                <View style={styles.modalView}>
                    <DatePicker
                        mode='calendar'
                        selected={date}
                        onDateChange={handleChangeDate}
                        maximumDate={endDate}
                        options={{
                            textHeaderColor: '#0F52BA',
                            textDefaultColor: '#0F52BA',
                            selectedTextColor: '#fff',
                            mainColor: '#0F52BA',
                            textSecondaryColor: '#0F52BA',
                        }}>
                    </DatePicker>
                    <TouchableOpacity onPress={handleOnPress}>
                        <Text style={styles.closeModalText}>{'Done'}</Text>
                    </TouchableOpacity>
                </View>
            </View>
        </Modal>
    </View>
)}
```

As can be seen, the component is mostly made out of the `<DatePicker>` components imported from the external libray and includes some other buttons to help handle the change of dates that user picked. 

The custom component takes 5 parameters, 
1. text = the form title for that corresponding input, in this case date of birth
2. openModal = the modal handler, stored in the parent page, to decide whether to open or close the date picker
3. handleOnPress = open the modal
4. date = the value for this form input, initially set to today's date
5. handleChangeDate = set the value of date to the state of Formik component once user has selected a date

### DropDownField Component

Another input form in the Self Input Form screen is made out of the `<DropDownField>` component which can be found in the `DropDownField.js` file under the `components` folder. Different from the `<InputTextField>` component, the dropdown field does not have a text input on the right side, but rather a selection of pre-defined options that users can select from.

We are using another external library to help build this component called `react-native-element-dropdown` so that we do not have to make this component from scratch. More information on the external library can be found **[here](https://www.npmjs.com/package/react-native-element-dropdown)**

```jsx
import { Dropdown } from 'react-native-element-dropdown';

const DropDownField = (text, value, data, setOnChange, handleOnFocus, errorMessage, onIconPress=null) => {
    return(
        <View>
            <View style={styles.optionView}>
                <View style={styles.inputTitleView}>
                    {onIconPress ?
                        <Icon name="help" color="#0F52BA" size='18' onPress={onIconPress}/> : null}
                    <Text style={styles.optionText}>{text}</Text>
                   
                </View>
                <Dropdown
                    style={styles.dropdown}
                    itemTextStyle={styles.itemStyle}
                    data={data}
                    maxHeight={300}
                    labelField="label"
                    valueField="value"
                    value={value}
                    onChange={item => {
                        setOnChange(item.value)
                    }}
                    onFocus={() =>{
                        handleOnFocus(true)
                        setOnChange(value)
                    }}
                />
            </View>
            <Text style={styles.errorText}>{errorMessage}</Text>
        </View>
    )
}
```

The key takeaway in this component is the `<Dropdown>` component in the right hand side of the form input, imported from the external library mentioned earlier. 

This custom component takes 5 parameters, 
1. text = the form title for that corresponding input
2. value = the current value of the selection
3. data = the list of available selection that will be shown in the dropdown, example shown below. Full list of data of the form can be found in `constants.js` file 
4. setOnChange = handler when selecting new value
5. handleOnFocus = handler when opening the dropdown
6. errorMessage = error message if any from the form validation
7. onIconPress = this is for showing more information when user needs more help when filling out the form

```jsx
import Constants from 'expo-constants';

const { manifest } = Constants;

const dietData = [
    { label: 'No Restrictions', value: 'Normal' },
    { label: 'Keto', value: 'Keto' },
    { label: 'Paleo', value: 'Paleo' },
    { label: 'Vegetarian', value: 'Vegetarian' },
    { label: 'Vegan', value: 'Vegan' },
    { label: 'Mediterranean', value: 'Mediterranean' },
    { label: 'Low Carb', value: 'Low Carb' },
    { label: 'No Sugar', value: 'No Sugar' },
];

...
```

### BMI and Age Field

Some field, bmi and age in specific is calculated automatically from other values of the form. We ensure that any error is handled, such as division by zero etc.

Age is calculated from the resulting user input of date of birth. Whereas BMI is calculated once user finish inputing heigh and weight. 

## Form Validation

Again, we are using `yup` object integrated in `Formik` to help with the validation. The schema is as following,

```jsx
const userInfoValidationSchema = yup.object().shape({
    firstName: yup
      .string()
      .required('Please enter your first name'),
    lastName: yup
      .string()
      .default('')
      .nullable(),
    dob: yup
      .date()
      .default(() => new Date())
      .required('Please enter your date of birth'),
})

const userHealthValidationSchema = yup.object().shape({
    insulin: yup
      .string()
      .required('Please select one'),
    cholesterol: yup
      .string()
      .required('Please select one'),
    bloodPressure: yup
      .string()
      .required('Please select one'),
    alcoholConsumption: yup
      .string()
      .required('Please select one'),
    smokingStatus: yup
      .string()
      .required('Please select one'),
    diet: yup
      .string()
      .required('Please select one'),
    sex: yup
      .string()
      .required('Please select one'),
    height: yup
      .string()
      .matches(/^\d*\.?\d*$/, { message: 'Height must be a number' })
      .required('Please enter your height in cm'),
    weight: yup
      .string()
      .matches(/^\d*\.?\d*$/, { message: 'Weight must be a number' })
      .required('Please enter your weight in kg'),
    bloodType: yup
      .string()
      .required('Please select one'),
})
```

## Submit User Information

Lastly, we have a `<Button>` component at the end of the form to submit the form data containing the user information into the database by communicating with the `Flask` backend server.

The function `sendData` will be called when the `onSubmit` event is triggered in the Formik component.

```jsx
import { createUser } from '../utils/api/user.api';

const sendData = async (newData) => {
    // console.log("NEW DATA:::",newData)
    const result = await createUser(newData)
    // console.log("EDIT HEALTH:::",result)
    if (!result.error){
        // console.log('CREATED USERR', JSON.stringify(result.data))
        navigation.navigate('Main')
    } else {
        // console.log(result.error)
        Alert.alert('Something went wrong. Please try again')
    }
}
```

`axios.post` method will be called to the `/user/<uid>` endpoint in the backend

```jsx user.api
...
const createUser = async (payload) => {
    try {
        const user = auth.currentUser.uid
        const response = await axios.post(`${flaskURL}/user/${user}`, payload, {
            headers: {
            'Content-Type': 'application/json'
            }
        });
        console.log('RESP',response)
        return { data: response.data, error: null }
    } catch (error) {
        // console.log(payload)
        // console.log('RESP',error.response)
        return { data: null, error }
    } 
};
...
```