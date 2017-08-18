#DRAFT - SUBJECT TO CHANGE


# React Native MeCard Contacts

React Native MeCard Contacts is a React Native API for accessing and manipulating the native device contact database. It is heavily influced by and borrows code from [react-native-contacts](hhttps://github.com/rt2zz/react-native-contacts). It was created to meet the needs of NiceToMeetMe, Inc.'s MeCard app and is available for use under the MIT License.

1. [Contact Object Property Types](#Contact_Object)
1. [Contact Object Properties](#Contact_Objects_Properties)
	* [Groups](#Groups)
	* [Example Contact Object](#Example_Contact_Object)
1. [Usage Examples](#Usage_Examples)
1. [API Reference](#API_Reference)
1. [Notes](#Notes)
1. [Installation](#Installation)
1. [License](#License)

## Contact Object
Each contact is held in a contact object. A contact object contain 4 types of properties: *label, label array, complex array, special*.

###labels
A string value. Each contact can only have one value for a given label.

```js
{
	familyName: 'Jung',
	givenName: 'Carl'
}
```

###label arrays
An array of objects, each with a label string, value string, and primary flag. Contact can have multiple labelled entries in a multilabel array. Primary may use false or undefined.

```js
{
	emailAddresses: [
		{
			label: 'home',
			value: 'home@home.com',
			primary: true
		},{
			label: 'work',
			value: 'work@work.com',
			primary: false
		}
	],
	
	websites: [
		{
			label: 'personal',
			value: 'www.mywebsite.com',
			primary: false
		}
	]
}
```

###complex arrays
An array of objects, each with a specific set of properties depending on the property name.  The specific complex object properties are listed below for each contact property.

```js
{
	notableDates: [
		{
			label: 'birthday',
			year: '1875',
			month: '07',
			day: '26'
		},{
			label: 'anniversary',
			year: '',
			month: '10',
			day: '22'
		}
	],
	
	postalAddresses: [
		{
			label: 'home',
			street: '123 N. Main St.',
			city: 'Anytown',
			region: 'CA',
			country: 'US',
			postcode: '90068',
			primary: true,
		}
	]
}
```

###special
Special properties are primarily private properties for use by the implementation, but may in some cases be useful.


##Contact Object Properties
All values are supported for reading and writing on iOS and Android except where noted.

```js
{
  recordID: '80f2f002ad56b4feae4652b5c05f4124eb',
  hasImage: true,

  namePrefix: 'Dr.',
  familyName: 'Jung',
  givenName: 'Carl',
  middleName: 'C.',
  nameSuffix: 'Sr.',
  nickName: 'Carl-o',
  phoenticGivenName: 'CAR-el',
  phoneticFamilyName: 'YUH-ng',

  company: 'Foomatics, Inc.',
  jobTitle: 'Developer',

  note: 'This is a note',

  //be sure to call getImage() before trying to use imageURI
  imageURI: {uri: 'file:///device/path/to/image.jpg'},

  phoneNumbers: [
    {
      label: 'mobile',
      value: '(555) 555-5555',
      primary: true,
    },{
      label: 'work',
      value: '(555) 555-5556',
    }
  ],

  emailAddresses: [
    {
      label: 'work',
      value: 'carl-jung@example.com',
    }
  ],

  websites: [
    {
      label: 'homepage',
      value: 'http://www.carljung.com/',
    }
  ],
  
  IMIDs: [
    {
      label: 'SKYPE',
      value: 'carl0612'
    }
  ],
  
  groups: [
  	{
      label: 'Friends',
      value: null //currently unused
  	}
  ],

  postalAddresses: [
    {
      label: 'work',
      street: '123 N Main Street',
      city: 'Chicago',
      region: 'IL', //Illinois in USA
      postcode: '12345-A234',
      country: 'US',
      primary: true
    }
  ],

  socialProfiles: [
    {
    	label: 'FACEBOOK',
    	URL: 'https://www.facebook.com/carl.jung.gustav/',
    	userID: 'carl.jung.gustav',
    	userName: 'Carl Jung',
    }
  ]

  notableDates: [
    {
      label: 'birthday',
      month: '6',
      day: '26',
      year: null,
    },{
      label: 'Carl Jung's death',
      month: '6',
      day: '6',
      year: '1961',
    }
  ]

}
```

###labels

| Property   | Description |
|------------|-------------|
| namePrefix | A name prefix such as Mr., Ms., Mme., Dr., etc
| familyName | Family name or "last name"
| givenName  | Given name or "first name"
| middleName | Middle name or names
| nameSuffix | A name suffix such as Jr., Sr., III, etc.es
| nickName   | Contact's nickname
| phoneticFamilyName | Phonetic representation of familyName **[iOS] _native contact manager will alphabetize contacts by phonetic if phonetic name fields are not empty_**
| phoneticMiddleName | Phonetic representation of middleName
| phoneticGivenName | Phonetic representation of givenName
| company    | Where the Contact works
| jobTitle   | Contact's job title
| note       | Note about contact. Appears in "Notes" on native Contact Manager

###label arrays
| Property   | Description |
|------------|-------------|
| phoneNumbers | One or more phone numbers. See [phoneNumbers](#phonenumbers)
| emailAddresses | One or more email addresses. See [emailAddresses](#emailaddresses)
| websites   | One or more website URLs. See [websites](#websites)
| IMIDs      | One or more IM service IDs. Primary is ignored. See [IMIDs](#IMIDs)
| groups     | A list of groups to which this contact belongs. Primary is ignored. See [groups](#groups)

###complex arrays
| Property   | Description |
|------------|-------------|
| postalAddresses| One or more physical street addresses. See [postalAddresses](#postaladdresses)
| socialProfiles | One or more social media IDs. see [socialProfiles](#socialProfiles)
| notableDates  | One or more important dates. See [notableDates](#notableDates) **[Android] _Not all contact managers show birthday, however value can be written, read, and synced._**

###special
| Property   | Type | Description |
|------------|------|-------------|
| hasImage   | boolean r/o | Does the contact have an associated image?
| imageURI | Object | A URL pointing to the contact's thumbnail image on the native device filesystem. See [Notes on adding and updating thumbnailPath](#notes-on-adding-and-updatring-thumbnailPath)
 recordID   | string | A hash used to reference this contact in the native contact manager database. Needed for updating and deleting contacts. Ignored for adding contacts. **[Android] _As of API 26, there is no unique persistent hash for contact records. When a contact is deleted, this hash value may be invalid or reference a different contact. See discussion [here](https://stackoverflow.com/a/41998956) for more info._** 

###Contact Object Property Details
#### phoneNumbers

A [label array](#label_arrays) containing one or more phone numbers. The following label fields are supported by iOS and Android.

| Label   | iOS | Android |
|---------|-----|---------|
| home      |:heavy_check_mark:| :heavy_check_mark:
| work      |:heavy_check_mark:| :heavy_check_mark:
| mobile      |:heavy_check_mark:| :heavy_check_mark:
| fax      |:heavy_check_mark:| :heavy_check_mark:
| (arbitrary)[1]| :heavy_check_mark: | :heavy_check_mark:
*[1]Arbitrary labels are allowed for phone numbers on both iOS and Android. The specific labels above are defined in each OS and may have special meaning.*

Notes: If more than one phone number is provided to addContact() in the array, and none or more than one have primary set to *true*, the number added as the primary contact number is undefined.

#### emailAddresses

A [label array](#label_arrays) containing one or more email addresses. The following label fields are supported by iOS and Android.

| Label   | iOS | Android |
|---------|-----|---------|
| home      |:heavy_check_mark:| :heavy_check_mark:
| work      |:heavy_check_mark:| :heavy_check_mark:
| mobile      |:heavy_check_mark:| :heavy_check_mark:
| (arbitrary)[1]| :heavy_check_mark: | :heavy_check_mark:
*[1]Arbitrary labels are allowed for email addresses on both iOS and Android. The specific labels above are defined in each OS and may have special meaning.*

Notes: If more than one email address is provided to addContact() in the array, and none or more than one have primary set to *true*, the address added as the primary email address is undefined.

#### websites

A [label array](#label_arrays) containing one or more website URLs. The following label fields are supported by iOS and Android.

| Label   | iOS | Android |
|---------|-----|---------|
| home      |:heavy_check_mark:| :heavy_check_mark:
| work      |:heavy_check_mark:| :heavy_check_mark:
| homepage  |:heavy_check_mark:| :heavy_check_mark:
| profile   |:heavy_check_mark:| :heavy_check_mark:
| blog      |:heavy_check_mark:| :heavy_check_mark:
| (arbitrary)[1]| :heavy_check_mark: | :heavy_check_mark:
*[1]Arbitrary labels are allowed for websites on both iOS and Android. The specific labels above are defined in each OS and may have special meaning.*

Notes: If more than one website is provided to addContact() in the array, and none or more than one have primary set to *true*, the website added as the primary website is undefined.

#### IMIDs

A [label array](#label_arrays) containing one or more IM serivces and IDs. The following label fields are supported by iOS and Android. Note pre-defined labels, as specific service names, are all-caps. Arbitrary labels may use mixed case.

| Label   | iOS | Android |
|---------|-----|---------|
| AIM        |:heavy_check_mark:| :heavy_check_mark:
| MSN        |:heavy_check_mark:| :heavy_check_mark:
| YAHOO      |:heavy_check_mark:| :heavy_check_mark:
| SKYPE      |:heavy_check_mark:| :heavy_check_mark:
| QQ         |:heavy_check_mark:| :heavy_check_mark:
| GOOGLETALK |:heavy_check_mark:| :heavy_check_mark:
| ICQ        |:heavy_check_mark:| :heavy_check_mark:
| JABBER     |:heavy_check_mark:| :heavy_check_mark:
| NETMEETING |:heavy_check_mark:| :heavy_check_mark:
| FACEBOOK   |:heavy_check_mark:| [1]
| GADUGADU   |:heavy_check_mark:| [1]
| (arbitrary)[1]| :heavy_check_mark: | :heavy_check_mark:
*[1]Arbitrary labels are allowed for IMIDs on both iOS and Android. The specific labels above, unless directed here, are defined in each OS and may have special meaning.*

Notes: If more than one IMID is provided to addContact() in the array, and none or more than one have primary set to *true*, the IMID added as the primary IMID is undefined.

#### groups
A [label array](#label_arrays) containing one or more group names in the 'label' property. The value property is currently unused but reserved for future use. See [Groups](#groups)

| Label   | iOS | Android |
|---------|-----|---------|
| (arbitrary)| :heavy_check_mark: | :heavy_check_mark:

#### postalAddresses

A [complex array](#complex_arrays) containing one or more physical addresses, contained in the following address object:

| Property Name   | Value Type | Description |
|------------|------------|-------------|
| label      | String     | (see below) |
| street     | String     | The complete street address ("123 N. Main Street Suite 500")
| city       | String     | The city in which the address resides.
| region     | String     | A location designation between *city* and *country*, if customary or necessary. For addresses in the USA this is the "state", in Canada the "province", etc. **[Android]: _maps to 'region'._ [iOS]: _maps to 'state'_** 
| country    | String     | The *ISO 3166-1 ALPHA-2* country code. Multilingual JSON files can be found [here](https://github.com/umpirsky/country-list)
| postcode   | String     | The postal code (zip code) for this address.
| primary    | boolean    | Default = *false* Indicates this postal address is the Contact's primary postal address. If more than one postal address is provided to addContact() in the array, and none or more than one have primary set to *true*, the address added as the primary postal address is undefined.

Address label support by platform:

| Label   | iOS | Android |
|---------|-----|---------|
| home      |:heavy_check_mark:| :heavy_check_mark:
| work      |:heavy_check_mark:| :heavy_check_mark:
| (arbitrary)[1]| :heavy_check_mark: | :heavy_check_mark:
*[1]Arbitrary labels are allowed for physical addresses on both iOS and Android. The specific labels above are defined in each OS and may have special meaning.*

#### socialProfiles

A [complex array](#complex_arrays) containing one or more social media profiles, contained in the following socialAccount object:

| Property Name   | Value Type | Description |
|------------|------------|-------------|
| label      | String     | (see below) |
| URL        | String     | The URL used to view this social profile
| userID     | String     | The user ID for the contact for this social media service
| userName   | String     | The contacts display name for this social media service

Social service label support by platform. Note pre-defined labels, as specific service names, are all-caps. Arbitrary labels may use mixed case.

| Label   | iOS | Android[1] |
|---------|-----|---------|
| FACEBOOK   |:heavy_check_mark:|[1]
| FLICKR     |:heavy_check_mark:|[1]
| LINKEDIN   |:heavy_check_mark:|[1]
| MYSPACE    |:heavy_check_mark:|[1]
| SINAWEIBO  |:heavy_check_mark:|[1]
| TENCENTWEIBO|:heavy_check_mark:|[1]
| TWITTER   |:heavy_check_mark:|[1]
| YELP      |:heavy_check_mark:|[1]
| STEAM |[3]|[1]
| QUORA |[3]|[1]
| GAMECENTER[2] |:heavy_check_mark:|
| (arbitrary)[3]| :heavy_check_mark:|
**_[1] As of API 26, Android does not support social media profiles in the contact database. React Native MeCard Contacts will add social contacts as websites on Android using the URL field, and will attempt to construct a socialProfile record from a website entry with a label that matches known social media sites. However, this socialProfile object will likely be incomplete_**

*[2] Apple GameCenter*

*[3] Arbitrary labels are allowed for social media profiles on iOS. The specific labels above, unless directed here, are defined in each OS and may have special meaning.*

#### notableDates

A [complex array](#complex_arrays) containing one or more notable dates, contained in the following notableDate object:

| Property Name   | Value Type | Description |
|------------|------------|-------------|
| label      | String | (see below) |
| year       | String | Year (Gregorian). Null for no year.
| month      | String | Month as a number (1-based index)
| day        | String | Day of the month as a number (1-based index)

Address label support by platform:

| Label   | iOS | Android |
|---------|-----|---------|
| birthday    |:heavy_check_mark:| :heavy_check_mark:
| anniversary |[1]| :heavy_check_mark:
| (arbitrary)[1]| :heavy_check_mark: | :heavy_check_mark:
*[1]Arbitrary labels are allowed for dates on both iOS and Android. The specific labels above, unless directed here, are defined in each OS and may have special meaning.*

### Groups

### Example Contact Object

##Usage Examples

##API Refernece

Both callback and Promise versions of the API are available and can be selected by importing the correct asset. _The callback API is provided to allow easier transtion from react-native-callback. If you are starting a new project, the Promise API is recommended._

Callback-based API: `RNMContacts = require('react-native-mecard-contacts')`

Promise-based API: `RNMContacts = require('react-native-mecard-contacts/async')`

####Prototype Objects
#####proto_Contact
An empty contact object, suitable for use as a contact record, prototype, or filter.

```js
{

  //LABELS
  namePrefix: '',
  familyName: '',
  givenName: '',
  middleName: '',
  nameSuffix: '',
  nickName: '',
  phoenticGivenName: '',
  phoneticFamilyName: '',
  company: '',
  jobTitle: '',
  note: '',

  //LABEL ARRAYS
  phoneNumbers: [],
  emailAddresses: [],
  websites: [],
  IMIDs: [],
  groups: [],

  //COMPLEX OBJECT ARRAYS
  postalAddresses: [],
  socialProfiles: [],
  notableDates: [],
  
  //SPECIAL
  imageURI: {},
  recordID: '',
  hasImage: false,
}
```

#####proto_postalAddress
```js
  proto_postalAddress = {
	street = '',
	city  = '',
	region = '',
	country = '',
	postcode = '',
	primary = false
  }
```
#####proto_socialProfile
```js
  proto_socialProfile = {
	label    = '',
	URL      = '',
	userID   = '',
	userName = ''
  }
```
#####proto_date
```js
  proto_date = {
	label = '',
	year  = '',
	month = '',
	day   = ''
  }
```
####Constants

Localization-isolated labels. Can be used to assure system specific labels are used when localization could be a concern.

| Label | Alias |
|----|----|
|`label_home`| home
|`label_work`| work
|`label_mobile`| mobile
|`label_fax`| fax
|`label_personal`| personal
|`label_birthday`| birthday
|`label_anniversary`| anniversary

####Functions

Unless noted, functions that operate using a contact object take an array of contact objects and will process all contacts passed in the array.

 
#####getAll( prototype=null )
Returns an array of all contacts in the database. Profile photos are not included, but must be requested using [getImages()](getImages( contact[] ))

| Arguments | |
|----|----|
|prototype| An object containing the keys to fetch. To fetch all known keys, pass proto_Contact or null (default = null). Any undefined keys will be excluded. Fetching only the needed keys increases the speed of querries.|

**Return Value** : An array off all contacts, each contact containing the keys provided in the prototype.

**Examples :**

```js
RNMContacts = require('react-native-mecard-contacts/async');
...

//get all contact details for all contacts on device
var contactList = RNMContacts.getAll();

//Only get the nickname of every contact
var contactList = RNMContacts.getAll( { nickname: '' } );

//Get all info except postal addresses
var contactList = RNMContacts.getAll( { ...RNMContacts, postalAddresses: undefined } );

//Get all contacts and then get all their photos
var contactList = RNMContacts.getImages( await RNMContacts.getAll() );
```

#####get( recordID, prototype=null )
Returns a single contact, identified using the contactID from a previous request.

| Arguments | |
|----|----|
| recordID | The contact recordID key from a previous request indicating which contact to get. (*may be brittle on Android*) 
|prototype| An object containing the keys to fetch. To fetch all known keys, pass proto_Contact or null (default = null). Any undefined keys will be excluded. Fetching only the needed keys increases the speed of querries.|

**Return Value** : A single contact object containing the keys provided in the prototype.

**Examples :**

```js
RNMContacts = require('react-native-mecard-contacts/async');
...

//get contact details for a specific contact
//the recordID was captured and stored in a previous session
var contactList = RNMContacts.get( cached.recordID, null );
```

#####getImages( contact[] )
Returns contact[] with valid imageURI keys, if the contact has a profile image

| Arguments | |
|----|----|
| contact[] | An array of contact objects for which the profile images should be fetched.

**Return Value** : Returns the same array reference passed in (not a copy)

**Examples :**

```js
RNMContacts = require('react-native-mecard-contacts/async');
...

//get contact details for a for all contacts
var contactList = await RNMContacts.getAll();

//ensure only the first contact has an imageURI, if it has an image in the database
await RNMContacts.getImages( [ contactList[0] ] );

//ensure all contact have an imageURI, if they have an image in the database
await RNMContacts.getImages( contactList );
```

#####`update( contact[] )`
#####`updateImages( contact[] )`
#####`add( contact[] )`
#####`find( neeedle_object, prototype )`
#####`delete( contact[] )`
#####`requestPermission()`
#####`getGroups()`
#####`addGroups( groupsNames[] )`

##Installation
**THIS SECTION IS NOT UP TO DATE**

run `npm install react-native-mecard-contacts`

### iOS
1. In XCode, in the project navigator, right click `Libraries` ➜ `Add Files to [your project's name]`
2. add `./node_modules/react-native-mecard-contacts/ios/RCTMContacts.xcodeproj`
3. In the XCode project navigator, select your project, select the `Build Phases` tab and in the `Link Binary With Libraries` section add **libRCTMContacts.a**

#### Permissions
As of Xcode 8 and React Native 0.33 it is now **necessary to add kit specific "permission" keys** to your Xcode `Info.plist` file, in order to make `requestPermission` work. Otherwise your app crashes when requesting the specific permission. I discovered this after days of frustration.

Open Xcode > Info.plist > Add a key (starting with "Privacy - ...") with your kit specific permission. The value for the key is optional.

You have to add the key "Privacy - Contacts Usage Description".

<img width="338" alt="screen shot 2016-09-21 at 13 13 21" src="https://cloud.githubusercontent.com/assets/5707542/18704973/3cde3b44-7ffd-11e6-918b-63888e33f983.png">

### Android
* In `android/settings.gradle`
```gradle
...
include ':react-native-mecard-contacts'
project(':react-native-mecard-contacts').projectDir = new File(settingsDir, '../node_modules/react-native-mecard-contacts/android')
```

* In `android/app/build.gradle`
```gradle
...
dependencies {
    ...
    compile project(':react-native-mecard-contacts')
}
```

* register module (in android/app/src/main/java/com/[your-app-name]/MainApplication.java)
```java
	...

	import com.ntmm.reactnativemecardcontacts.ReactNativeMeCardContacts; 	// <--- import module!

	public class MainApplication extends Application implements ReactApplication {
	    ...

	    @Override
	    protected List<ReactPackage> getPackages() {
	      return Arrays.<ReactPackage>asList(
	        new MainReactPackage(),
	        new ReactNativeMeCardContacts() 	// <--- and add package
	      );
	    }

    	...
    }
```
If you are using an older version of RN (i.e. `MainApplication.java` does not contain this method (or doesn't exist) and MainActivity.java starts with `public class MainActivity extends Activity`) please see the [old instructions](https://github.com/rt2zz/react-native-contacts/tree/1ce4b876a416bc2ca3c53e7d7e0296f7fcb7ce40#android)

* add Contacts permission (in android/app/src/main/AndroidManifest.xml)
(only add the permissions you need)
```xml
...
  <uses-permission android:name="android.permission.READ_CONTACTS" />
  <uses-permission android:name="android.permission.WRITE_CONTACTS" />
  <uses-permission android:name="android.permission.READ_PROFILE" />
...
```


## LICENSE

[MIT License](LICENSE)Or1g1n0r1g1n
