---
sidebar_position: 5
---

import genetics from '../img/genetics.png';

# Genetics.js

The file `Genetics.js` contains <strong>works to be implemented in the future</strong> if the project is to be pursued. What has been done is the inital UI design (prototype) of what is to be implemented in the future. The UI design of the Genetics screen can be seen in the Figure below.

<img src={genetics} width="250" style={{border: '1px solid #ddd', display: 'block', marginLeft: 'auto', marginRight: 'auto'}}/>
<br/>

As can be seen from the image, the initial plan is to have a screen which is able to show a summary of health related diseases of the user according to the user's data. This way the user would be able to see the risk of different diseases in one simple page. 

In addition to the CHD risk data, we also planned to include other health disease risk such as stroke and diabetes/ To do this, we planned to extract genetics data of the user that have been collected from third party provider such as 23andme (or other provider) to get the detailed genetics data of the user.

## Reason Not Implemented
However, this was not implemented due to the limited timeframe of the project. During the implementation phase of the project, it was then found out that the API that 23andme provides is not open to public and app developer must apply for invitation to be able to access the API. During the project the timeline, the application for the API access was already closed and hence it was no longer possible for us to pursue this feature. 

Furthermore, since this is not the key feature of the application. We decided that it is best that this feature is implemented later on in the future instead. 

## Progress
Aside from the prototype UI design, the UI of the Genetics page have actually been implemented. However, the UI is not being shown in the application since the feature is not implemented yet. The only thing that has been implemented is the UI components of the page. Explained in more detail below. 

```jsx
<Text style={{...styles.heading,fontSize: 20, marginLeft: 20, marginTop: 10}}>Carrier Status</Text>
<View style={{ flexDirection: "row", flexWrap: "wrap", alignItems: 'center', justifyContent: 'center'}}>
    <CarrierStatusCard disease="Coronary Heart Disease" />
    <CarrierStatusCard disease="Stroke" />
    <CarrierStatusCard disease="Type 2 Diabetes" status={false} />
</View>
```

The first main part of the code (shown above) in Genetics.js shows the health disease risk summary of the user. This will need to be made dynamic in the future by adding logic to fetch data from HTTP API call to the respective endpoints. 

```jsx
<Text style={{...styles.heading,fontSize: 20, marginLeft: 20, marginTop: 10}}>Disease Risks</Text>
<View style={{ flexDirection: "row", flexWrap: "wrap-reverse", alignItems: 'center', justifyContent: 'center'}}>
    <DataCard title="Diabetes" numbers="32.6" units="%" width={'42%'} note="Average: 30.1%"/>
    <DataCard title="Cancer" numbers="2.2" units="%" width={'42%'} note="Average: 1.1%"/>
    <DataCard title="CHD" numbers="22.3" units="%" width={'42%'} note="Average: 20.9%"/>
    <DataCard title="Stroke" numbers="29.6" units="%" width={'42%'} note="Average: 30.3%"/>
</View>
```

The next main part another card which shows the actual value of the summary of the health disease risk of the user shown in the card above. Again, the next step would be to add logic to fetch dynamic data from the corresponding data source, which is the third party provider of these data. 



