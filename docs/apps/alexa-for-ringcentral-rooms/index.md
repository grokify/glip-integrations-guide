# Alexa for RingCentral Rooms and Office 365 Installation

## Overview

You can configure Echo devices with the RingCentral Rooms for Alexa skill, and then deploy them into new or existing RingCentral Rooms across your organization. After the devices are configured, participants in RingCentral Rooms can start, join, and end RingCentral meetings by speaking to Alexa.

## Prerequisites

* Credentials to a master account that has access to the following:
  * Office 365, including ability to update and approve calendar requests
  * RingCentral account with permission to edit RingCentral Rooms
  * Amazon Web Services (AWS), including access to Alexa for Business (A4B) web portal (https://aws.amazon.com/alexaforbusiness/)
  * Amazon web portal (https://www.amazon.com)
* Windows 7 or later computer (not a VM) running the following software (required for the Device Setup Tool):
  * AWS Tools for Windows PowerShell.
  * NET Framework 3.5 or later.
* RingCentral Rooms for Mac version 4.1.20278.0206 or higher
* RingCentral Rooms for PC Version 4.1.22620.0319 or higher
* Amazon Echo, Echo Dot, or Echo Plus. See the Managing Devices section of the Alexa for Business Administration Guide for a information about supported devices.
* A wifi connection to the internet that can be accessed by both the Windows computer and the Echo device


### Office 365 Prerequisite

For Office 365 you will need the following:

* Access to Exchange such as Office 365 Enterprise E1, E2 or E3
* A RingCentral Rooms Service Account (needs license)
* A “Resource” Room mailbox (does not need a license)

We will cover how to add the service account and room mailbox below:

#### Create RC Room Service Account - needs license

This user needs to update and approve calendar requests.

This user needs an Office 365 licenses.

A simple way to do this can be performed with the following steps:

1. navigate to the "Microsoft 365 admin center"
2. navigate to `Home` > `Active users` and then click `+ Add a user`
3. set a Display name and Username. The Username can be somthing like `rcrooms`
4. set an appropriate role. If you don't have a custom role to use, you can use “Global adimistrator” for testing purposes.
5. select a license
6. click `Add`

#### RingCentral Rooms Prerequisite


1. Navigate to `Admin Centers` > `Exchange` > `Recipients` > `Resources` > `+` > `Room mailbox`
2. Enter the required fields, "Room name" and "Email address" and click "Save"
3. Under "booking delegates" click "Accept or decline booking requests automatically"
4. Under "mailbox delegation" grant "Full Access" to the service account desired
