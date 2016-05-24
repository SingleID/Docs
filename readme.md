
![SingleID Official Logo](http://www.singleid.com/img/logonew.png)

	THIS WHITE-PAPER IS IN FLUX AND IS SUBJECT TO CHANGE AT ANY TIME
	Please DO NOT RELY upon it until this notice has been removed.
	(Which should be soon!)

---


## Abstract

SingleID is a cloudless and data-decentralized identity provider with a privacy-preserving approach.
SingleID doesn't reinvent signature, encryption, channel binding, or client verification. It relies completely on SSL for some degree of confidentiality and server authentication.
It provides a dramatically better user experience and at the same time a higher security than existing password­-based platforms for identifying and authenticating users as it does not require a central trusted third party. SingleID also automates form-filling with a single-­click based on a pre-aggregated master form stored in the user's smartphone app. 


## Latest revision of this White Paper

Status: Draft
Latest update: 2015-10-04

---

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED",  "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC 2119](https://www.ietf.org/rfc/rfc2119.txt) .

---


# What is SingleID

[SingleID](https://www.singleid.com) is a [Patented](http://www.freepatentsonline.com/y2015/0200780.html) identity management and authentication platform providing users with a smartphone app to organize their personal data that can be accessed by any third party needing to identify that user by being furnished with their SingleID. 

In a way SingleID is a mobile data wallet that pre-aggregates different kinds of data – personal data, billing data, shipping data, identification data (passport, driver’s license), membership data, contract data, etc. – so that they can be released with a single click from your smartphone upon request by a third party.

And all of this is done with a zero knowledge[^zero-knowledge] architecture and state-of-the art encryption, observing the highest privacy and security standards. SingleID is a distributed platform and thus no database of sensitive personal data is being built up anywhere.

Computer break-ins are happening more frequently than ever before. Our mission is to make the targets of the attack worthless to the attacker.

[^zero-knowledge]: In cryptography, a zero-knowledge proof or zero-knowledge protocol is a method by which one party,the prover, can prove to another party, the verifier, that a given statement is true, without conveying any information apart from the fact that the statement is indeed true.

Decentralization of data breaks the old paradigm based on trusted third party.

This document also describes how to apply this information and the ways in which a device "SingleID compliant" must communicate in response to requests.

Data can be requested from any recipient system that knows the SingleID of a device and that are connected to the Internet.

The data can be saved in the device, manually by the user or by reading a code QrCode whose structure is described in this document. The data are aggregated, managed and sent in the manner described herein.

## Assurance level

At SingleID we divide the spectrum of accounts (and so their security requirements) in two levels:

- Throw-away accounts
- Sensitive Accounts



## SingleID sequence chart

The following diagram demonstrate how SingleID is not only a digital wallet and a form-filling software, but is also a two step authenticator without the need to store personal data on any "third party server".


### Requirements
1. The user already has the App SingleID with a profile saved
2. The recipient system has the plugin installed as described in this White-Paper



### Definition and meaning
Term         | Meaning
----------------- | -------
Recipient System  | Tipically a web site with the official web plugin installed
SingleID Server   | Any server owned from SingleID Inc. that is compliant with the Server Protocol described herein
data request      | The type of data that the recipient system is asking for ( personal , billing, shipping ... )
User Device  | A device with the SingleID App

## Throw-away accounts

Users might create **Throw-away accounts** on the spur of the moment for testing, participating in a pseudo-anonymous conversation thread, or making a one-time purchase without saving payment credentials but with provision for checking order status. Maybe this is the most boring user experience also because is the most frequently. We can give a completely friction-less experience on this type of account working as 1-factor (something-you-have) authentication.

### Sequence for throw-away accounts

![Throw-away accounts](https://dl.dropboxusercontent.com/u/10636650/screenshot-markdown-SingleID/github/throw-away-accounts.png)


### Please note 
1. A disposable, one time session token is randomly generated at the beginning from the Recipient system ( UTID ) and is trasmitted to the SingleID Server, that will forward also the UTID to the user's device.
2. No personal data has been transmitted over the SingleID Server
3. The SingleID server doesn't know also if the user accept or decline to send the data to the recipient system
4. The recipient System will receive not only the personal data from the user's device, but also the UTID. This is the proof that the user's device where the data come from is really in the hands of the person who start the request

## Sensitive Account


**Sensitive accounts** include an individual’s primary email or online banking accounts. Here, loss of data, either by deletion or public exposure, is commonly found to have severe and sometime unforeseen consequences. This type of account needs only a special first handshake with your SingleID compliant device

### Sequence for first handshake between device and relying party

![First-handshake sensitive accounts](https://dl.dropboxusercontent.com/u/10636650/screenshot-markdown-SingleID/github/sensitive-accounts-handshake.png)



### Sequence for Sensitive Account (after first handshake)

![Flow sensitive accounts](https://dl.dropboxusercontent.com/u/10636650/screenshot-markdown-SingleID/github/sensitive-accounts-flow.png)







---

## Server Protocol

This paragraph is about the server protocol version until version 1.0

The official server url is https://app.singleid.com




## Allowed interaction with SingleID Server

There are only 6 allowed action for exchange information with the SingleID Server.

Five are from a user's device

	1. getnewSingleID
	2. bringbackmypwd
	3. tellmemore
	4. updatepushID
	5. setmyrecoveryemail


One is from the recipient system

	1. askfordata


## What happens in a User's Device

![App flow](https://dl.dropboxusercontent.com/u/10636650/screenshot-markdown-SingleID/github/App-flow.png)

# Interactions between Device and SingleID Server

Each step below reported are already coded in the official App. This White-Paper is only to explain how the App Works and how is simple and powerful the logic behind.

The official App is available on the [Android play store](https://play.google.com/store/apps/details?id=com.singleid.wapp), on the Apple [App Store](https://itunes.apple.com/us/app/singleid/id957303337) and is also available for [Windows Phone 8](http://www.windowsphone.com/it-it/store/app/singleid/2b9570c4-09eb-45db-8bab-37ba3cab9c06) on the Microsoft Store with the name "SingleID".

When you open the App for the first time, the app do these steps:

 - Create a random string of 32 hexadecimal char called TOKEN
 - Retrieve the PushID from the own Push Server ( Android, iOs, WP8 )
 - Send these data to the SingleID Server asking for a new, and never used before, SingleID ( an 8 digit hexadecimal string )
 - Wait for a Push notification with a login request
 
## First Action: getnewSingleID
The first interaction that the app must do with the SingleID server is the getnewSingleID

The App have to send the following POST DATA, over SSL, to the SingleID Server

PLATFORM

Permitted values are:
Value  | Details
---- | -------
GCM  | for Android device
APNS | for iOs device
WM8  | for Windows Phone 8 device

REGISTRATION_ID

Is the "Push id" of the Device
Value  | Details
---- | -------
RegistrationId  | for Android device
AppleId | for iOs device
XXX  | for Windows Phone 8 device

	
TOKEN

	Must be a random string of 32chars hexadecimal created from the app
Tecnically speaking this is the unique value that could identify a device.
**Remember that we do not rely on UUID for privacy reason.**

DETECTED_DEVICE_LANGUAGE

2 char code (from [iso 639 standard](http://en.wikipedia.org/wiki/ISO_639))
todate allowed value are: it, en, bg, de.
 
APP_VERSION

A value between 1.0 and 1.2 ( at 2016-05 )
	
DEVICE_TYPE

Permitted values are:
|Value  | Details
|---- | -------
|smartphone  | for stat purposes only
|tablet | for stat purposes only
|wearable  | for stat purposes only
|testing | for testing purposes
	
ACTION_ID

	Permitted value is:
	getnewSingleID
	


	
### Response to a correct getnewSingleID request

The response of the server, in case of a correct request, must be a Json string with the following scheme:


	{
	"SIDVer": "0.8.2",
	"Reply": "ok",
	"SingleID": "b1625ebe",
	"Password": "b6f2a6539c290deb6c0b4a11707216c6",
	"Recovery Key": "a73ca9905a5d600095da15f862a82a22",
	"PopupTitle": "Welcome",
	"Popup": "Now you have an unique SingleID",
	"PopupButtonLabel": "Enjoy",
	"PopupButtonUrl": "#"
	}


SIDVer
	The Software Server version.
	
Reply
	The status of the request. Could be 'ok' or 'ko'
	
SingleID
	The SingleID that the Server has associated to the TOKEN sent before. We identify a device from his TOKEN
	
Password
	All the personal data stored inside the device must be encrypted with AES 256 with this password. The password must not be saved into the device.

Recovery Key
	This is a secret value that had to be stored inside the device for future purposes
	
Popup(*)
	Are string values that could be displayed to the user from the App
	
---

	
## Subsequent requests from an already registered device
	
When the App start, and a SingleID and a TOKEN are already stored, the App has to request back from the server the password to be able to decrypt the data stored inside.

In order to be SingleID compliant the App must sent the following POST DATA, over SSL, to the SingleID Server


|Var Name  | Value
|---- | -------
|ACTION_ID  | bringbackmypwd
|SingleID | `$SingleID`
|TOKEN  | `$TOKEN`
|DETECTED_DEVICE_LANGUAGE | Used for stat purpose only ( two char code as [iso 639 standard](http://en.wikipedia.org/wiki/ISO_639))
|APP_VERSION  | Used for stat purpose only



### Response to a correct bringbackmypwd request

The response of the server in case of a correct request will be a Json string like the following

	{
	"SIDVer": "0.8.2",
	"Reply": "ok",
	"Password": "a3f2a6539a290deb6c0b4a11707216c7",
	"PopupTitle": "",
	"Popup": "",
	"PopupButtonLabel": "",
	"PopupButtonUrl": ""
	}


SIDVer
	the Software Server version.
	
Reply
	the status of the request. Could be 'ok' or 'ko'
	
Password
	All the data stored inside the device must be encrypted with AES 256 with this password. The password must not be saved into the device.

Popup(*)
	Are string values that should be displayed to the user

--- 

## Check for queued Request

When receiving a Push Notification, the device must check the SingleID Server for queued authentication request

The payload of a push notification is too small to contain all the data so when a push notification reach the device, the device will make a REST CALL to the server with the following value:

|Var Name  | Value
|---- | -------
|ACTION_ID  | tellmemore
|SingleID | `$SingleID`
|PASSWORD  | `$PASSWORD`
|DETECTED_DEVICE_LANGUAGE | Used for stat purpose only ( two char code as ISO  [iso 639 standard](http://en.wikipedia.org/wiki/ISO_639))
|APP_VERSION  | Used for stat purpose only





### Response with only one pending request
     
	[
	{
	"SingleID": "30d37d54",
	"Date": "2014-10-23T15:24:37+01:00",
	"Name": "BetaTesting Inc.",
	"Logo_url": "http:\/\/www.singleid.com\/img\/logonew.png",
	"url_waiting_data": "http:\/\/www.singleid.com\/dv1\/plugin.php",
	"requested_data_group": "1",
	"ssl": "0",
	"UTID": "e6b9973ea39ee28a88b687afaa1835c9"
	}
	]

---

SingleID
	The device SingleID. Is included for future purposes only.
	
Date
: 	Date of the request ( [ISO 8601 standard](http://en.wikipedia.org/wiki/ISO_8601) )
	
Name
	Name of the recipient system. Should be displayed to the user.
	
Logo_url
	Logo of the recipient system. Should be displayed to the user.

url_waiting_data
	The url where the app must send the data if the user accepts.
	
requested_data_group
	Which type of data the recipient system is asking for. The user must not change this value.

Permitted values are:
|Value  | Meaning
|---- | -------
|1  | Personal Data / Company Data
|1,2,3 | Personal, Billing and Shipping data
|1,-2,3  | Personal, Billing and Shipping data ( Without credit card )
|1,2,3,4 | Personal, Billing, Shipping and Identification data
|1,-2,3,4 | Personal, Billing ( Without credit card ), Shipping and Identification data
|1,4,5 | First handshake ( Sensitive account )
|1,4,6 | Subsequent authorization requestes ( Sensitive account )
	
ssl
: 	Must be 0 for http or 1 for https ( related to url_waiting_data )

UTID
	Acronym for Unique Transaction ID. Must be a hexadecimal 32char string. Is generated randomly from the recipient system at the beginning of the request.
	The value must be sent with the personal data to the `url_waiting_data`
	
--- 
	
	
	

### Response with more than one pending request


	[
	{
	"SingleID": "30d37d54",
	"Date": "2014-10-23T15:24:37+01:00",
	"Name": "BetaTesting Inc.",
	"Logo_url": "http:\/\/www.singleid.com\/img\/logonew.png",
	"url_waiting_data": "http:\/\/www.singleid.com\/dv1\/plugin.php",
	"requested_data_group": "1",
	"ssl": "0",
	"UTID": "e6b9973ea39ee28a88b687ffaa1835c9"
	},
	{
	"SingleID": "30d37d54",
	"Date": "2014-10-23T15:28:56+01:00",
	"Name": "BetaTesting Inc.",
	"Logo_url": "http:\/\/www.singleid.com\/img\/logonew.png",
	"url_waiting_data": "http:\/\/www.singleid.com\/dv1\/plugin.php",
	"requested_data_group": "1",
	"ssl": "0",
	"UTID": "f7b0666f7a2354a52018c73682648222"
	},
	{
	"SingleID": "30d37d54",
	"Date": "2014-10-23T15:28:59+01:00",
	"Name": "BetaTesting Inc.",
	"Logo_url": "http:\/\/www.singleid.com\/img\/logonew.png",
	"url_waiting_data": "http:\/\/www.singleid.com\/dv1\/plugin.php",
	"requested_data_group": "1",
	"ssl": "0",
	"UTID": "aaa0666f7a2350a47018c3131d5fe821"
	}
	]
	


### Response without queued request
                    
	{
	"SIDVer":"0.8.2",
	"Reply":"ok",
	"PopupTitle":"",
	"Popup":"",
	"PopupButtonLabel":"",
	"PopupButtonUrl":""
	}
                    
### Response in case of error
      
	{
	"SIDVer":"0.8.2",
	"Reply":"ko",
	"PopupTitle":"",
	"Popup":"Something goes wrong...",
	"PopupButtonLabel":"",
	"PopupButtonUrl":""
	}
                    


## Update Push Notification ID ( for iOs e WP8 device only )

Each App installed on a smartphone has an internal "PushID". This means that if you want remotely wake app a specific app, you can gently ask to Apple|Google|Microsoft to forward a push notification to a specific App. 

On Android the "PushID" related to an App doesn't change also if you remove the App and reinstall it.

On other platform ( Apple and Microsoft ) the "PushID" could change.

In these cases, in order to receive push Notification, the App should update the device information in the following way. 
The App must sent the following POST DATA, over SSL, to the SingleID Server


|Var Name  | Value
|---- | -------
|ACTION_ID  | updatepushID
|SingleID | `$SingleID`
|PASSWORD  | `$PASSWORD`
|DETECTED_DEVICE_LANGUAGE | Used for stat purpose only ( two char code as ISO  [iso 639 standard](http://en.wikipedia.org/wiki/ISO_639))
|APP_VERSION  | Used for stat purpose only
|NEW_REGISTRATION_ID | The new pushid to use

**Note:** iOS device tokens must be 64 hexadecimal characters



### Response to a correct updatepushID request

	{
	"SIDVer":"0.8.2",
	"Reply":"ok",
	"PopupTitle":"",
	"Popup":"",
	"PopupButtonLabel":"",
	"PopupButtonUrl":""
	}

### Response in case of wrong request
      
	{
	"SIDVer":"0.8.2",
	"Reply":"ko",
	"PopupTitle":"Ops!",
	"Popup":"Something goes wrong...",
	"PopupButtonLabel":"",
	"PopupButtonUrl":""
	}
	


## How enable the 'Remote Lock' possibility

---
Any SingleID could be remotely locked only if a user executes this step **before the loss** of the device.

---


The App must sent the following POST DATA, over SSL, to the SingleID Server

|Var Name  | Value
|---- | -------
|ACTION_ID  | setmyrecoveryemail
|SingleID | `$SingleID`
|PASSWORD  | `$PASSWORD`
|DETECTED_DEVICE_LANGUAGE | Used for stat purpose only ( two char code as [iso 639 standard](http://en.wikipedia.org/wiki/ISO_639))
|APP_VERSION  | Used for stat purpose only
|Salt  | A Random hexadecimal string of 16 char created from the App
|HashedEmail  | The Md5 hash of (Salt + Email). In this way we do not store the email of the user but only an hash of the email. So we cannot contact the user but the user could remotely lock the device if he fill the form at https://app.singleid.com/remotelock/ 

Note:
We are working on including bcrypt algorithm to replace md5 asap.

--- 

![Remote Lock](https://dl.dropboxusercontent.com/u/10636650/screenshot-markdown-SingleID/github/SingleID-Remote-Lock.png)

### Response to a correct setmyrecoveryemail request

	{
	"SIDVer": "0.8.2",
	"Reply": "ok",
	"PopupTitle": "Email saved",
	"Popup": "If you want to remotely lock this App visit www.singleid.com",
	"PopupButtonLabel": "How to",
	"PopupButtonUrl": "http:\/\/www.singleid.com\/faq\/"
	}
	
### Response in case of error
      
	{
	"SIDVer":"0.8.2",
	"Reply":"ko",
	"PopupTitle":"Ops!",
	"Popup":"Something goes wrong...",
	"PopupButtonLabel":"",
	"PopupButtonUrl":""
	}
	
	
---


# How to request data from a Device 

The simple way to request data from a SingleID device is with a REST CALL to the SingleID Server.

This can be done with a plugin on the recipient system.

If for recipient system you are thinking to a website you can use the official SingleID "web plugin" button .
 
If for recipient system you intend something different like an ATM or a cash register you have only to follow the specific below.

For "web plugin" we intend an html code that had to be embedded on the form page as an iframe. This is the easiest way to make each recipient system SingleID compliant.

Alternative versions of the plugin are welcome as they will follow the request scheme described in this document.


Another way to read the data stored in a SingleID Device is reading a QrCode.
The QrCode specs are written below.


![SingleID Flow](https://dl.dropboxusercontent.com/u/10636650/screenshot-markdown-SingleID/github/Flow.png)


## Plugin Installation

At 2015-10 we have two official Plugin.


To use the **Official SingleID Web Plugin** on your site you must have in the same folder of you web form a file called plugin.php

The latest version of "plugin.php" is hosted, on [github](https://github.com/SingleID/web-plugin)

For convenience, in this White-Paper, we will call the web form page as form.php



### Requirements of the web plugin

form.php must have jquery
If you don't use jquery on that page you have to add with this line of code:


	<script src="https://ajax.googleapis.com/ajax/libs/jquery/1.6.1/jquery.min.js"></script>



The next step is to insert the SingleId Button.

Place this code where you want to display the button:


	<iframe src="SingleID.php?op=init" width="290" height="80" frameborder="0"></iframe>


---

PLEASE NOTE:

	You need also to create a folder called *userdata/* and make it writable from plugin.php
	Please make sure that this folder is not browsable. For better security we recommended to create an empty index.html in this folder

---



Before testing you have to define 5 constants at the beginning of plugin.php


LOGO_URL

	This is the full HTTP URL of your logo
	This logo will be displayed on the App when you will asking a request to the user.
	The logo will be showed inside a rectangle of 100x80 px so be careful with dimension and size in kb
	The logo should not have any trasparency.

example:

	define("LOGO_URL", "http://www.singleid.com/img/logonew.png");


---
Really Important:

	- if the image is not reachable from web the push notification to the user will not be sent and will silenty fail !
	- The url MUST NOT be on https://
	- The url MUST NOT be over 50kb

---
	
SITE_NAME
 
	This is the label that the user will see on the SingleID App when they receive a Push notification.
	
example:

	define("SITE_NAME", "test name from whitepaper");


*PLEASE NOTE* that this value must be shorter than 24 character

---

Requested_data

	This is the type of data that you could ask to a device.
These are the allowed values and their meaning

|String Value  | Meaning
|---- | -------
|1  | Personal Data / Company Data
|1,2,3 | Personal, Billing and Shipping data
|1,-2,3  | Personal, Billing and Shipping data ( Without credit card )
|1,2,3,4 | Personal, Billing, Shipping and Identification data
|1,-2,3,4 | Personal, Billing ( Without credit card ), Shipping and Identification data
|1,4,5 | First handshake ( Sensitive account )
|1,4,6 | Subsequent authorization requestes ( Sensitive account )
	

example

	define("requested_data", "1,2,3");

	


---
**PLEASE NOTE:**

You are free to ask the set "1" with or without https.
	
In order to ask for the other set you MUST USE SSL for both form.php and plugin.php


---


A login request should appear in this way

![example of login request](https://dl.dropboxusercontent.com/u/10636650/screenshot-markdown-SingleID/data-request.png)
	
	
billing_key

	You are free to ask the set "1" with or without this field filled.
	In order to ask for the other set you MUST type in this field the billing_key.
	You can obtain a billing key on the website www.singleid.com 

The billing key is a secret value and had to keep hidden
 
admin contact
: 	Is an optional field. If isset we will sent to this email address an alert if something goes wrong between the plugin and our server.

We will send only 1 alert each 24 hours and only if the domain of the e-mail is the same of the domain where plugin.php is hosted.
	
	
## Latest step: Matching the input data with your existing form

If you look the code for an Array called **$arrTranslation** you will see the following lines:

	$arrTranslation['Pers_title'] 				    = '';
	$arrTranslation['Pers_first_name'] 			= '';
	$arrTranslation['Pers_middle_name'] 			= '';
	$arrTranslation['Pers_last_name'] 			    = '';
	$arrTranslation['Pers_birthdate'] 			    = '';
	$arrTranslation['Pers_gender'] 				= '';
	$arrTranslation['Pers_postal_street_line_1'] 	= '';
	$arrTranslation['Pers_postal_street_line_2'] 	= '';
	$arrTranslation['Pers_postal_street_line_3'] 	= '';
	$arrTranslation['Pers_postal_city'] 			= '';
	$arrTranslation['Pers_postal_postalcode'] 		= '';
	$arrTranslation['Pers_postal_stateprov'] 		= '';
	$arrTranslation['Pers_postal_countrycode'] 		= '';
	$arrTranslation['Pers_telecom_fixed_phone'] 		= '';
	$arrTranslation['Pers_telecom_mobile_phone'] 		= '';
	$arrTranslation['Pers_first_email'] 			= '';
	$arrTranslation['Pers_skype'] 				= '';
	$arrTranslation['Pers_first_language'] 			= '';
	$arrTranslation['Pers_second_language'] 		= '';
	$arrTranslation['Pers_contact_preferred_mode'] 		= '';
	$arrTranslation['Pers_newsletter_agree'] 		= '';
	$arrTranslation['Pers_billing_first_name'] 		= '';
	$arrTranslation['Pers_billing_middle_name'] 		= '';
	$arrTranslation['Pers_billing_last_name'] 		= '';
	$arrTranslation['Pers_billing_telecom_fixed_phone'] 	= '';
	$arrTranslation['Pers_billing_telecom_mobile_phone']	= '';
	$arrTranslation['Pers_billing_email'] 			= '';
	$arrTranslation['Pers_billing_vat_id'] 			= '';
	$arrTranslation['Pers_billing_fiscalcode'] 		= '';
	$arrTranslation['Pers_invoice_required'] 		= '';
	$arrTranslation['Company_type'] 			= '';
	$arrTranslation['Company_name'] 			= '';
	$arrTranslation['Company_registration_number'] 		= '';
	$arrTranslation['Company_website'] 			= '';
	$arrTranslation['Comp_postal_street_line_1'] 		= '';
	$arrTranslation['Comp_postal_street_line_2'] 		= '';
	$arrTranslation['Comp_postal_street_line_3'] 		= '';
	$arrTranslation['Comp_postal_city'] 			= '';
	$arrTranslation['Comp_postal_postalcode'] 		= '';
	$arrTranslation['Comp_postal_stateprov'] 		= '';
	$arrTranslation['Comp_postal_countrycode'] 		= '';
	$arrTranslation['Comp_contact_title'] 			= '';
	$arrTranslation['Comp_contact_first_name'] 		= '';
	$arrTranslation['Comp_contact_middle_name'] 		= '';
	$arrTranslation['Comp_contact_last_name'] 		= '';
	$arrTranslation['Comp_contact_qualification'] 		= '';
	$arrTranslation['Comp_contact_department'] 		= '';
	$arrTranslation['Comp_contact_telecom_fixed_phone'] 	= '';
	$arrTranslation['Comp_contact_telecom_fax'] 		= '';
	$arrTranslation['Comp_contact_telecom_mobile_phone']	= '';
	$arrTranslation['Comp_contact_email'] 			= '';
	$arrTranslation['Comp_contact_skype'] 			= '';
	$arrTranslation['Comp_contact_language'] 		= '';
	$arrTranslation['Comp_contact_second_language'] 	= '';
	$arrTranslation['Comp_contact_preferred_mode'] 		= '';
	$arrTranslation['Comp_contact_newsletter_agree'] 	= '';
	$arrTranslation['Comp_billing_first_name'] 		= '';
	$arrTranslation['Comp_billing_middle_name'] 		= '';
	$arrTranslation['Comp_billing_last_name'] 		= '';
	$arrTranslation['Comp_billing_telecom_phone_number']	= '';
	$arrTranslation['Comp_billing_email'] 			= '';
	$arrTranslation['Comp_billing_vat_id'] 			= '';
	$arrTranslation['Comp_billing_fiscalcode'] 		= '';
	$arrTranslation['Ecom_payment_card_name'] 		= '';
	$arrTranslation['Ecom_payment_card_type'] 		= '';
	$arrTranslation['Ecom_payment_card_number'] 		= '';
	$arrTranslation['Ecom_payment_card_expdate_month'] 	= '';
	$arrTranslation['Ecom_payment_card_expdate_year'] 	= '';
	$arrTranslation['Ecom_payment_card_verification'] 	= '';
	$arrTranslation['Ecom_payment_card_visa_verified'] 	= '';
	$arrTranslation['Ecom_payment_mode'] 			= '';
	$arrTranslation['Ecom_shipto_postal_name_prefix'] 	= '';
	$arrTranslation['Ecom_shipto_postal_company_name'] 	= ''; // business
	$arrTranslation['Ecom_shipto_post_office_box'] 		= ''; // business
	$arrTranslation['Ecom_shipto_postal_name_first'] 	= '';
	$arrTranslation['Ecom_shipto_postal_name_middle'] 	= '';
	$arrTranslation['Ecom_shipto_postal_name_last'] 	= '';
	$arrTranslation['Ecom_shipto_postal_street_line1'] 	= '';
	$arrTranslation['Ecom_shipto_postal_street_line2'] 	= '';
	$arrTranslation['Ecom_shipto_postal_street_line3'] 	= '';
	$arrTranslation['Ecom_shipto_postal_floor'] 		= '';
	$arrTranslation['Ecom_shipto_postal_city'] 		= '';
	$arrTranslation['Ecom_shipto_postal_postalcode'] 	= '';
	$arrTranslation['Ecom_shipto_postal_stateprov'] 	= '';
	$arrTranslation['Ecom_shipto_postal_countrycode'] 	= '';
	$arrTranslation['Ecom_shipto_contact_phone'] 		= '';
	$arrTranslation['Ecom_shipto_phone_number_for_shipper'] = ''; // business
	$arrTranslation['Ecom_shipto_note'] 			= '';
	$arrTranslation['Ident_name_prefix'] 			= '';
	$arrTranslation['Ident_name_first'] 			= '';
	$arrTranslation['Ident_name_last'] 			= '';
	$arrTranslation['Ident_birthdate'] 			= '';
	$arrTranslation['Ident_gender'] 			= '';
	$arrTranslation['Ident_country_of_birth'] 		= '';
	$arrTranslation['Ident_country_of_citizenship'] 	= '';
	$arrTranslation['Ident_country_where_you_live'] 	= '';
	$arrTranslation['Ident_passport_number'] 		= '';
	$arrTranslation['Ident_passport_issuing_country'] 	= '';
	$arrTranslation['Ident_passport_issuance'] 		= '';
	$arrTranslation['Ident_passport_expiration'] 		= '';
	$arrTranslation['Ident_identity_card_number'] 		= '';
	$arrTranslation['Ident_identity_card_issued_by'] 	= '';
	$arrTranslation['Ident_identity_card_issuance'] 	= '';
	$arrTranslation['Ident_identity_card_expiration'] 	= '';
	$arrTranslation['Ident_driver_license_number'] 		= '';
	$arrTranslation['Ident_driver_license_issued_by'] 	= '';
	$arrTranslation['Ident_driver_license_issuance'] 	= '';
	$arrTranslation['Ident_driver_license_expiration'] 	= '';





The Key of the array are the fields name that will be sent from the App ( and that you cannot change ) . 

For each key you have to set as value the id of the input field present in your form.

Example #1

If you have a form like this


	<label>First Name</label>
	<input name="Name" id="id_name" type="text">
	<label>Last Name</label>
	<input name=lastname" id="id_lastname" type="text">
	...


You have to set the following values in $arrTranslations


	$arrTranslation['Pers_title'] 		    = 'id_name';
	$arrTranslation['Pers_first_name'] 	= 'id_lastname';
	...


Of course you can change only the fields name in which you are interested.



 
## Plugin interaction with SingleID Server

When as user input in the plugin a 8 digit hex value  ( a Valid SingleID )

the plugin have to send, **server side**, a data request to the SingleID's Server over SSL


The data request must contain the following POST data

---

SingleID

	The value typed in the plugin

UTID

	A random value. must be an Md5 or a hex 32 char length

LOGO_URL

	described above

SITE_NAME

	described above

requested_data

	already described

ssl 

	could be 1 or 0 if the requested_data is = 1. otherwise must be 1

url_waiting_data
 
	The url where the app had to send the data

ACTION_ID
	must be "askfordata"


--- 

### Please note 

In the official plugin these step are already defined and they are correctly self-filled. They are here explained only to help you to understand each step.

--- 



### Server Reply

The Server, in case of correct data should respond as follow
 

	{
	  "SIDVer": "0.8.6",
	  "Reply": "ok",
	  "PopupTitle": "Good",
	  "Popup": "Your Request has been forwarded",
	  "PopupButtonLabel": "",
	  "PopupButtonUrl": "#"
	}



In case of error should response with following json



	{
	  "SIDVer": "0.8.6",
	  "Reply": "ko",
	  "PopupTitle": "Wrong",
	  "Popup": "I don't like your SingleID",
	  "PopupButtonLabel": "",
	  "PopupButtonUrl": "#"
	}


 
 
### What's next ?

The server must verify if the sender is allowed to send the request. 

There are formal check and sostantial check to do here.

If the sender is allowed a push notification will be sent to the SingleID device.





## Time to close the circle

Device now knows:

1. Who is asking for data
2. Which type of data are looking for
3. The UTID ( or OTP as you prefer )
4. Where sent the values.


If the user decide to send the data, all the input name will be sent to the plugin.php located at the "url_waiting_data" with a single [POST CALL](http://en.wikipedia.org/wiki/POST_%28HTTP%29)


## How the plugin will fill the fields now

Since the plugin has been included as iframe, with a simple javascript (that is bundled with the web plugin) is now possible to assign the received value to the elements in the parent document.


	if(input_type == 'text')
	{
		parent.window.jQuery('#'+key).val(value);
	}
	else if(input_type == 'radio')
	{
		parent.window.jQuery('input[name="'+key+'"][value="'+value+'"]').prop("checked", true);
	}
	else if(input_type == 'checkbox')
	{
		parent.window.jQuery('input[class="'+key+'"][value="'+value+'"]').prop("checked", true);
	}
	else
	{
		parent.window.jQuery("#"+key).val(value);
	}

### Which data the plugin will receive from device 

When `requested_data_group` is set to 1 the plugin will receive the following data 

#### Personal profile

|requested_data_group  | Profile Type | POST var name | value
|---- | ------- | --- | --- | 
|1  | personal | UTID | [random one time token]
|1  | personal | which_set | `"personal"`
|1  | personal | Pers_title | user defined
|1  | personal | Pers_first_name | user defined
|1  | personal | Pers_middle_name | user defined
|1  | personal | Pers_birthdate | user defined
|1  | personal | Pers_gender | user defined
|1  | personal | Pers_postal_street_line_1 | user defined
|1  | personal | Pers_postal_street_line_2 | user defined
|1  | personal | Pers_postal_street_line_3 | user defined
|1  | personal | Pers_postal_city | user defined
|1  | personal | Pers_postal_postalcode | user defined
|1  | personal | Pers_postal_stateprov | user defined
|1  | personal | Pers_postal_countrycode | user defined
|1  | personal | Pers_telecom_fixed_phone | user defined
|1  | personal | Pers_telecom_mobile_phone | user defined
|1  | personal | Pers_first_email | user defined
|1  | personal | Pers_skype | user defined
|1  | personal | Pers_first_language | user defined
|1  | personal | Pers_second_language | user defined
|1  | personal | Pers_contact_preferred_mode | user defined
|1  | personal | Pers_newsletter_agree | user defined


#### Business profile

|requested_data_group  | Profile Type | POST var name | value
|---- | ------- | --- | --- | 
|1  | business | UTID | [random one time token]
|1  | business | which_set | `"business"`
|1  | business | Company_type | user defined
|1  | business | Company_name | user defined
|1  | business | Company_registration_number | user defined
|1  | business | Company_website | user defined
|1  | business | Comp_postal_street_line_1 | user defined
|1  | business | Comp_postal_street_line_2 | user defined
|1  | business | Comp_postal_street_line_3 | user defined
|1  | business | Comp_postal_city | user defined
|1  | business | Comp_postal_postalcode | user defined
|1  | business | Comp_postal_stateprov | user defined
|1  | business | Comp_postal_countrycode | user defined
|1  | business | Comp_contact_title | user defined
|1  | business | Comp_contact_first_name | user defined
|1  | business | Comp_contact_middle_name | user defined
|1  | business | Comp_contact_last_name | user defined
|1  | business | Comp_contact_qualification | user defined
|1  | business | Comp_contact_department | user defined
|1  | business | Comp_contact_telecom_fixed_phone | user defined
|1  | business | Comp_contact_telecom_fax | user defined
|1  | business | Comp_contact_telecom_mobile_phone | user defined
|1  | business | Comp_contact_email | user defined
|1  | personal | Comp_contact_skype | user defined
|1  | personal | Comp_contact_language | user defined
|1  | personal | Comp_contact_second_language | user defined
|1  | business | Comp_contact_preferred_mode | user defined
|1  | business | Comp_contact_newsletter_agree | user defined


                            

### Date format
Each date field is stored in SingleID with the [ISO8601](http://it.wikipedia.org/wiki/ISO_8601) format
For example the field Pers_birthdate will have the following value "YYYY-MM-DD"
But all the date input will be sent to recipient system also splitted in three var with the following suffix

	_day
	_month
	_year

So if you need the separated value you can read directly these var instead of split the first one

	// example
	Pers_birthdate_day
	Pers_birthdate_month
	Pers_birthdate_year
	

### Credit card format

The field called **Ecom_payment_card_number** is stored and sent to recipient system with only digits as [ISO7812](http://en.wikipedia.org/wiki/ISO/IEC_7812)

### Phone Number Format

Are allowed only digits and '+'


### Select List
 
**Be careful !**

The value on a select list will be assigned from Javascript row like this

	parent.window.jQuery("#"+key).val(value);

To avoid error you have to double check that the value that should be sent from the app are included in the possible values inside your select list!

The field that are stored as select list in the official app ( so with a fixed value ) are the following

|POST Name  | Possible values ( comma separated ) | More info
|---- | ------- | -----	
|which_set  | personal,business | Personal profile / Business profile
|Pers_title | Mr, Mrs | 
|Pers_gender  | M, F | Male or Female
|Pers_postal_countrycode| two uppercase char value  | [ISO 3166](http://www.theodora.com/country_digraphs.html)
|Pers_first_language| two lowercase char value  | [ISO 3166](http://www.theodora.com/country_digraphs.html)
|Pers_second_language| two lowercase char value  | [ISO 3166](http://www.theodora.com/country_digraphs.html)
| Pers_contact_preferred_mode | email, phone working hours, phone in the evening | Self-esplicative
| Pers_newsletter_agree | Y, N | Yes or No
| Ecom_payment_card_type | AMER, BANK, DC, DINE, DISC, JCB, MAST, NIKO, NSPK, SAIS, UC, UCAR, VISA, Vtron | American Express, Bankcard, DC, Diners Club, Discover, JCB, Mastercard, Nikos, PRO 100, Saison, UC, Ucard, Visa, Visa Electron
|Ecom_payment_mode | Credit card 1, Credit card 2, paypal | The user has selected the first credit card stored in his profile; or the second one; The user will pay the transaction with paypal so prepare the redirect | 

 



 



That's all folks



	

# Qrcode import / export

The data stored into a device could be easily imported/exported with QrCode.
If you have more than one device, for example, you could fill a profile on the first device and then export all the written data to another device in a very simple way.
On the first device you have to create a Qrcode. On the second device you have to enable the camera and grab the QrCode. A profile copy will be created on the second device.
A QrCode coul be very useful also in the retail world and / or where there isn't internet connection



## QRcode protocol

Data are written as a serialized string of an array.

	// example
	var QrCodeText = JSON.stringify(ProfileData);
	
In case of personal profile will be used the array called QrPersonal with the following key:value pair. 
In case of a business profile will be used the array called QrBiz.

Below a valid "stringified" string

	{"0":"web","1":"p","2":"Mr","3":"Daniele","5":"Vantaggiato","6":"1981-01-01","9":"M","10":"plaza square","13":"Venice","14":"30100","15":"VE","16":"IT","18":"0000000","19":"daniel@singleid.com","21":"it","22":"en","23":"email","24":"N","40":"Daniele","42":"Vantaggiato","44":"0000000","45":"daniel@singleid.com","49":"Daniele Vantaggiato","90":"Mr","91":"Daniele","93":"Vantaggiato","94":"plaza square","98":"Venice","99":"30100","100":"VE","101":"IT","120":"Mr","121":"Daniele ","122":"Vantaggiato","123":"1981-01-01","126":"M","127":"IT","128":"IT","129":"IT"}'


Before creating the Qrcode the string must be compressed with [LZ77 algorithm](http://en.wikipedia.org/wiki/LZ77_and_LZ78) so the above string will be the following compressed string in a QrCode SingleID 1.0 compliant

	"String: {"0":"web","1":"p","2":"Mr","3":"Daniele","5":"Vantaggiato","6":"1981-01-01","9":"M","10":"plaza square","13":"Venic` ) 4":"30100","1` v E","16":"IT","18":"00000` =!9":"d`!G @singleid.com","21":"it","22":"en","23":"email","24":"N","40`"$(42`"!,44`!)(45` }449`"s% `"i)90`#9#91`#5(9`"^ ` >)4`"x-98`"~'99`"~'00`#"$01`##$2`!)$1`"i `!P#` 7 `"4-1`"z `$M(126`$T#27` n%8` y%9`$4!}"
	
The resulting QrCode appear as:

---


![SingleID QrCode 1.0 compliant](https://dl.dropboxusercontent.com/u/10636650/screenshot-markdown-SingleID/QrCode.png)

---

#### QrPersonal


	Name:0,
	which_set:1,
	Pers_title:2,
	Pers_first_name:3,
	Pers_middle_name:4,
	Pers_last_name:5,
	Pers_birthdate:6,
	Pers_gender:9,
	Pers_postal_street_line_1:10,
	Pers_postal_street_line_2:11,
	Pers_postal_street_line_3:12,
	Pers_postal_city:13,
	Pers_postal_postalcode:14,
	Pers_postal_stateprov:15,
	Pers_postal_countrycode:16,
	Pers_telecom_fixed_phone:17,
	Pers_telecom_mobile_phone:18,
	Pers_first_email:19,
	Pers_skype:20,
	Pers_first_language:21,
	Pers_second_language:22,
	Pers_contact_preferred_mode:23,
	Pers_newsletter_agree:24,
	Pers_billing_first_name:40,
	Pers_billing_middle_name:41,
	Pers_billing_last_name:42,
	Pers_billing_telecom_fixed_phone:43,
	Pers_billing_telecom_mobile_phone:44,
	Pers_billing_email:45,
	Pers_billing_vat_id:46,
	Pers_billing_fiscalcode:47,
	Pers_invoice_required:48,
	Ecom_payment_card_name_1:49,
	Ecom_payment_card_type_1:50,
	Ecom_payment_card_number_1:51,
	Ecom_payment_card_expdate_month_1:52,
	Ecom_payment_card_expdate_year_1:53,
	Ecom_payment_card_verification_1:54,
	Ecom_payment_card_visa_verified_1:55,
	Ecom_payment_card_name_2:56,
	Ecom_payment_card_type_2:57,
	Ecom_payment_card_number_2:58,
	Ecom_payment_card_expdate_month_2:59,
	Ecom_payment_card_expdate_year_2:60,
	Ecom_payment_card_verification_2:61,
	Ecom_payment_card_visa_verified_2:62,
	Ecom_payment_bancomat_name_on_card:63,
	Ecom_payment_bancomat_type:64,
	Ecom_payment_bancomat_account_number:65,
	Ecom_payment_bancomat_Bankleitzahl:66,
	Ecom_payment_bancomat_card_number:67,
	Ecom_payment_bancomat_expdate_month:68,
	Ecom_payment_bancomat_expdate_year:69,
	Ecom_payment_bancomat_PIN:70,
	Ecom_payment_bank_name:71,
	Ecom_payment_bank_account_owner:72,
	Ecom_payment_bank_IBAN:73,
	Ecom_payment_bank_BIC:74,
	Ecom_payment_bank_account_number:75,
	Ecom_payment_bank_Sort_code:76,
	Ecom_payment_mode:77,
	Ecom_shipto_postal_name_prefix:90,
	Ecom_shipto_postal_name_first:91,
	Ecom_shipto_postal_name_middle:92,
	Ecom_shipto_postal_name_last:93,
	Ecom_shipto_postal_street_line1:94,
	Ecom_shipto_postal_street_line2:95,
	Ecom_shipto_postal_street_line3:96,
	Ecom_shipto_postal_floor:97,
	Ecom_shipto_postal_city:98,
	Ecom_shipto_postal_postalcode:99,
	Ecom_shipto_postal_stateprov:100,
	Ecom_shipto_postal_countrycode:101,
	Ecom_shipto_contact_phone:102,
	Ecom_shipto_note:103,
	Ident_name_prefix:120,
	Ident_name_first:121,
	Ident_name_last:122,
	Ident_birthdate:123,
	Ident_gender:126,
	Ident_country_of_birth:127,
	Ident_country_of_citizenship:128,
	Ident_country_where_you_live:129,
	Ident_passport_number:130,
	Ident_passport_issuing_country:131,
	Ident_passport_issuance:132,
	Ident_passport_expiration:135,
	Ident_identity_card_number:138,
	Ident_identity_card_issued_by:139,
	Ident_identity_card_issuance:140,
	Ident_identity_card_expiration:143,
	Ident_driver_license_number:146,
	Ident_driver_license_issued_by:147,
	Ident_driver_license_issuance:148,
	Ident_driver_license_expiration:151};



#### QRbiz


	Name:0,
	which_set:1,
	Company_type:2,
	Company_name:3,
	Company_registration_number:4,
	Company_website:5,
	Comp_postal_street_line_1:6,
	Comp_postal_street_line_2:7,
	Comp_postal_street_line_3:8,
	Comp_postal_city:9,
	Comp_postal_postalcode:10,
	Comp_postal_stateprov:11,
	Comp_postal_countrycode:12,
	Comp_contact_title:13,
	Comp_contact_first_name:14,
	Comp_contact_middle_name:15,
	Comp_contact_last_name:16,
	Comp_contact_qualification:17,
	Comp_contact_department:18,
	Comp_contact_telecom_fixed_phone:19,
	Comp_contact_telecom_fax:20,
	Comp_contact_telecom_mobile_phone:21,
	Comp_contact_email:22,
	Comp_contact_skype:23,
	Comp_contact_language:24,
	Comp_contact_second_language:25,
	Comp_contact_preferred_mode:26,
	Comp_contact_newsletter_agree:27,
	Comp_billing_first_name:40,
	Comp_billing_middle_name:41,
	Comp_billing_last_name:42,
	Comp_billing_telecom_phone_number:43,
	Comp_billing_email:44,
	Comp_billing_vat_id:45,
	Comp_billing_fiscalcode:46,
	Ecom_payment_card_name_1:47,
	Ecom_payment_card_type_1:48,
	Ecom_payment_card_number_1:49,
	Ecom_payment_card_expdate_month_1:50,
	Ecom_payment_card_expdate_year_1:51,
	Ecom_payment_card_verification_1:52,
	Ecom_payment_card_visa_verified_1:53,
	Ecom_payment_card_name_2:54,
	Ecom_payment_card_type_2:55,
	Ecom_payment_card_number_2:56,
	Ecom_payment_card_expdate_month_2:57,
	Ecom_payment_card_expdate_year_2:58,
	Ecom_payment_card_verification_2:59,
	Ecom_payment_card_visa_verified_2:60,
	Ecom_payment_bancomat_name_on_card:61,
	Ecom_payment_bancomat_type:62,
	Ecom_payment_bancomat_account_number:63,
	Ecom_payment_bancomat_Bankleitzahl:64,
	Ecom_payment_bancomat_card_number:65,
	Ecom_payment_bancomat_expdate_month:66,
	Ecom_payment_bancomat_expdate_year:67,
	Ecom_payment_bancomat_PIN:68,
	Ecom_payment_bank_name:69,
	Ecom_payment_bank_account_owner:70,
	Ecom_payment_bank_IBAN:71,
	Ecom_payment_bank_BIC:72,
	Ecom_payment_bank_account_number:73,
	Ecom_payment_bank_Sort_code:74,
	Ecom_payment_mode:75,
	Ecom_shipto_postal_name_prefix:90,
	Ecom_shipto_postal_company_name:91,
	Ecom_shipto_postal_name_first:92,
	Ecom_shipto_postal_name_middle:93,
	Ecom_shipto_postal_name_last:94,
	Ecom_shipto_post_office_box:95,
	Ecom_shipto_postal_street_line1:96,
	Ecom_shipto_postal_street_line2:97,
	Ecom_shipto_postal_street_line3:98,
	Ecom_shipto_postal_floor:99,
	Ecom_shipto_postal_city:100,
	Ecom_shipto_postal_stateprov:101,
	Ecom_shipto_postal_postalcode:102,
	Ecom_shipto_postal_countrycode:103,
	Ecom_shipto_phone_number_for_shipper:104,
	Ecom_shipto_note:105,
	Ident_name_prefix:120,
	Ident_name_first:121,
	Ident_name_last:122,
	Ident_birthdate:123,
	Ident_gender:126,
	Ident_country_of_birth:127,
	Ident_country_of_citizenship:128,
	Ident_country_where_you_live:129,
	Ident_passport_number:130,
	Ident_passport_issuing_country:131,
	Ident_passport_issuance:132,
	Ident_passport_expiration:135,
	Ident_identity_card_number:138,
	Ident_identity_card_issued_by:139,
	Ident_identity_card_issuance:140,
	Ident_identity_card_expiration:143,
	Ident_driver_license_number:146,
	Ident_driver_license_issued_by:147,
	Ident_driver_license_issuance:148,
	Ident_driver_license_expiration:151




















