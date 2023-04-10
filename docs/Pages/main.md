---
sidebar_position: 5
---

# Main.js

The user will be redirected to the `Main.js` screen after login, signup and filling in the validation form. In this screen, a check will be done if a user has filled in the required personal information or not. If they have not done so, the user will be redirected to the `SelfInputForm` screen again to fill in the details first before able to access the Main screen.

## Tab, TabView and TabComponent

The Main screen is actually made up of a `<TabView>` container component. This component is imported from the `rneui` UI library to allow us to integrate it with the Bottom Navigation Bar used for switching screens. Read more about it **[here](https://reactnativeelements.com/docs/components/tab)**

This allows us to render multiple different screens at the same time, while showing the one selected by the navigation bar to be shown to the screen. This also allows the screen to have **sliding behaviour**. The `<TabView>` container will then host 3 different screens in our screen, namely, `Home`, `Risk`, and `Community`. We assign the screens in the `<TabView.Item>` component with the children being the screen component to be rendered.

```jsx
return (
    <>
        <TabView value={index} onChange={setIndex} disableSwipe={true} disableTransition={true}>
            <TabView.Item style={{width: '100%'}}>
                <Home headerSubtitle={"Your daily statistics"} navigation={navigation}/>
            </TabView.Item>
            <TabView.Item style={{width: '100%'}}>
                <Risk headerTitle={"10-Year CHD Risk"} headerSubtitle={""} navigation={navigation} focused={index == 2}/>
            </TabView.Item>
            <TabView.Item style={{width: '100%'}}>
                <Community headerTitle={"Welcome to Community"} headerSubtitle={"Your home for fitness related news, local events, connecting with friends, and discovering groups"} navigation={navigation}/>
            </TabView.Item>
        </TabView>

        <TabComponent
            value={index}
            containerStyle={styles.container}
            onChange={(e) => setIndex(e)}
            disableIndicator={true}
            variant="primary">
            <TabComponent.Item
                title="Home"
                buttonStyle={styles.buttonStyle}
                titleStyle={styles.textStyle}
                icon={<Icon color='#FFFFFF' name='home' size={ iconSize }></Icon>}/>
            <TabComponent.Item
                title="Risk"
                buttonStyle={styles.buttonStyle}
                titleStyle={styles.textStyle}
                icon={<Icon color='#FFFFFF' name='heartbeat' type='font-awesome' size={ iconSize }></Icon>}/>
            <TabComponent.Item
                title="Community"
                buttonStyle={styles.buttonStyle}
                titleStyle={styles.textStyle}
                icon={<Icon color='#FFFFFF' name='groups' size={ iconSize }></Icon>}/>
        </TabComponent>
    </>
);
```

As explained, this allows us to have a Navigation Bar for the main screens of the application too using the `<TabComponent>` component exported from the same library. The `onChange` handler will be used to `setIndex` of the active screen. The index is assigned based on the order of the `<TabView.Item>` in the `<TabView>` component. In this case, Home will be index 0, Risk index 1, and Community index 2.

## Checking User Data

To check user data, we use the `useEffect` React hook, which is the first function to be runned when the screen is rendered.

```jsx
async function checkUserData() {
    try {
        let user = auth.currentUser?.uid
        const response = await axios.get(`${flaskURL}/user/verifyData/${user}`);
        let hasData = response.data
        if (hasData) {
            return
        } else {
            navigation.push("Self Input Form")
        }
    } catch(error) {
        return { data: null, error }
    }
}

useEffect(() => {
    checkUserData()
}, [])
```
We use the `auth` variable imported from `config.js` again to get the logged in user uid. Then we communicate with our `flask` backend and check the corresponding response if the response indicate used does not have data (have not submitted the self input form). Push the `SelfInputForm.js` screen so that user can first fill in their data.