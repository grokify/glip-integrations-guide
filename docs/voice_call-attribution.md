# Call Attribution Integration Guide

:warning: **DRAFT** :warning:

## Overview

Attribution is the ability to identify the source of a phone call, typically to evaluate the effectiveness of various ad campains that have a call to action (CTA) that involves making a phone call.

This is done by using a different phone number, called a Call Tracking Number (CTN), for each data source. An example data source can be a Google ad.

Numbers can be deployed in ads 2 ways:

* CTNs written directly into text and display ads
* CTNs dynamically written on to websites using JavaScript replacing Default Numbers with CTNs

Attribution can be deployed with or without a direct RingCentral integration.

1. Without a direct integration, the attribution system will provide it's own CTNs and then forward those CTNs to RingCentral Default Numbers (DNs), aka destination numbers.
1. With a direct integration, RingCentral will provide the CTNs and no forwarding is necessary. The CTNs will be assigned to the call queues or extensions directly so no forwarding is necessary. Default numbers are still used for website addresses and as a way to associate CTNs to each other.

The rest of this document describes the second approach with direct integration with RingCentral Office.

## Assumptions

1. Customers of RingCentral using a DNI system will pre-provision two sets of phone numbers:
  1. A set of Default Numbers for web properties and to associate CFNs with each other. These numbers should exist without the attribution system.
  1. A set of CFNs which are associated with Default Numbers.

## Overview

1. Initial Setup
  1. Deploy a set of standard destination numbers (SDNs), each for a call queue (CQ)
  1. Deploy a set of call tracking numbers (CTNs) for use with the attribution system
  1. Assign the CTNs to the relevant CQs
  1. Set the CTN's `label` property to include the SDN
  1. Configure the CQ to send the Phone Number Name (aka `label`) in notification events
1. Number Insertion
  1. Download phone numbers for each CQ
  1. Use CTNs in ads
  1. For websites, map CTNs to SDNs and replace SDN on web page
1. Handle incoming calls
  1. Subscribe for telephony events for CQ extensions
  1. Extract CTN and SDN numbers from `from.phoneNumber` and __ respectively
  1. Store `sessionId` and `recordingId` to download CDR and call recording
1. Download CDR and Recording


## Create a Pool of Labeled Numbers

1. Manually create the set of DNs and CTNs
1. From a event subscription management perspetive, it may be useful group the CTNs by extension since subscriptions can be made at the extension level.
1. Use the update phone number API to add the DN as the `phoneNumber.label`

```
PUT /restapi/v1.0/account/{accountId}/phone-number/{phoneNumberId}

{
    "label" : "Company`s secret number"
}
```


## Assign Numbers to Queues

Queue should be designed to use propagage "Phone Number Name"


## Listening to Incoming Calls

### Create Subscription for Phone Calls

1. Create a subscription to listen to incoming calls for the account. Event filter:

https://developers.ringcentral.com/api-reference#Subscriptions

```
POST /restapi/v1.0/subscription

{
    "eventFilters": [
        "/restapi/v1.0/account/~/extension/~/presence?detailedTelephonyState=true"
    ],
    "deliveryMode": {
        "transportType": "PubNub",
        "encryption": "true"
    }
}
```

### Events

The Event will have a few things of interest.

`from`
`from.number`
`to`
`to.number`
`to.mailboxId`
`uiCallInfo`
`uiCallInfo.primary.value`


When you get events, you can see something like the following. The phone numbe rname property will be in the `uiCallInfo` property.


{  
   "body":{  
      "parties":[  
         {  
            "from":{  
               "number":"+13055551212",
               "name":"Zillow.com"
            },
            "state":{  
               "missedCall":"true",
               "name":"Disconnected"
            },
            "to":{  
               "mailboxId":"400130811008",
               "name":"Sales",
               "number":"+18774443333"
            },
            "uiCallInfo":{  
               "primary":{  
                  "type":"InDIDLabel",
                  "value":"+14155550100"
               },
               "additional":{  
                  "type":"None"
               }
            }
         }
      ]
   },
   "event":"Notify_CallSession",
   "version":"2.4"
}
Options


### Runtime

1. For each incoming call event, retrieve the incoming phone number from `body.parties[x].from.phoneNumber`
1. Map phone number to campaignId in `phoneNumber.label` using cache or phone number search API
1. For each incoming call event, save `sessionId` and `recordingId` to retrieve additional info.

APIs:

* Account Telephony Sessions Event Reference:
  * https://developers.ringcentral.com/api-reference#Account-Telephony-Sessions-Event-Beta
* Search Company Directory Entries
  * https://developers.ringcentral.com/api-reference#Company-Contacts-searchDirectoryEntries

## Retrieving an Associated Call Recording

Pull the recordings:

https://stackoverflow.com/questions/55071991/how-can-i-retrieve-a-ringcentral-call-recording-from-a-monitored-incoming-call