---
title: Set up an iOS or iPadOS device in Shared Device Mode
description: Learn how to set up an iOS or iPadOS device in Shared Device Mode
author: Dickson-Mwendia
manager: CelesteDG
ms.author: dmwendia
ms.date: 08/19/2024
ms.service: msal
ms.subservice: msal-ios-mac
ms.reviewer: henrymbugua, akgoel
ms.topic: install-set-up-deploy
ms.custom: sfi-image-nochange
#Customer intent: 
---

# Setup an iOS or iPadOS device in Shared Device Mode 

[Shared device mode](/entra/identity-platform/msal-shared-devices) allows you to configure an iOS 14+ or iPadOS device to be more easily and securely shared by employees. Employees can sign-in once and get single sign-on (SSO) to all apps that support this feature, giving them faster access to information. When they're finished with their shift or task, they can sign out of the device through any supported app that also signs them out from all apps that support shared device mode. The device will then be ready for use by the next employee without allowing access to previous user's data.  

## Prerequisites

- An Azure account with an active subscription. If you don't have one, [create an account for free](https://azure.microsoft.com/free/).
- An iOS device with iOS 14+ or later that isn't registered with Microsoft Entra ID. If it's registered, reset it to factory settings.
- The latest version of [Microsoft Authenticator app](https://play.google.com/store/apps/details/Microsoft_Authenticator?id=com.azure.authenticator&hl=en_NZ) installed on the device.
- For manual setup, the device should be managed by a Mobile Device Management (MDM) tool such as Microsoft Intune.  
- For setup via MDM, the device should be managed by an MDM that supports shared device mode.

## Zero-touch setup via Intune 

Microsoft Intune supports zero-touch provisioning for devices in Microsoft Entra shared device mode, which means that the device can be set up and enrolled in Intune with minimal interaction from the frontline worker.  

To set up a device in shared device mode when using Microsoft Intune as the MDM, first step is to enroll the shared device into Intune and install Authenticator app with Shared Device Mode enabled. For more information, see how to [Set up enrollment for devices in Microsoft Entra shared device mode](/mem/intune/enrollment/automated-device-enrollment-shared-device-mode).  

Once enrolled, launch the Authenticator app, which will initiate and complete the registration process. Alternatively, you can open any of the shared device mode enabled applications, which will launch the Authenticator app and complete registration.  
 
## Manual setup

### Enable the Single Sign On Plug-in  

Use the following information to enable the SSO plug-in by using MDM.

#### Microsoft Intune configuration

If you use Microsoft Intune as your MDM service, you can use built-in configuration profile settings to enable the Microsoft Enterprise SSO plug-in:

1. Configure the [SSO app plug-in](/mem/intune/configuration/use-enterprise-sso-plug-in-ios-ipados-with-intune) settings of a configuration profile. 
1. If the profile isn't already assigned, [assign the profile to a user or device group](/mem/intune/configuration/device-profile-assign).

The profile settings that enable the SSO plug-in are automatically applied to the group's devices the next time each device checks in with Intune.

#### Manual configuration for other MDM services

If you don't use Intune for MDM, you can configure an Extensible Single Sign On profile payload for Apple devices. Use the following parameters to configure the Microsoft Enterprise SSO plug-in and its configuration options.

iOS settings:

- **Extension ID**: `com.microsoft.azureauthenticator.ssoextension`
- **Team ID**: This field isn't needed for iOS.

### Configure the Microsoft Authenticator app

To configure the Authenticator app settings and register the device with Entra ID, the Cloud Device Administrator in a tenant should follow these steps:

1. Launch the Authenticator App.
1. On the Shared Device Mode manual setup UI, enter the organizational email and select “Register as Shared Device**, as shown:

    :::image type="content" source="media/share-device-mode/microsoft-authenticator-set-up-shared-device-mode.png " alt-text="Screenshot showing the device registration screen in app":::

1. The Authenticator app prompts you to provide your credentials, as shown:

    :::image type="content" source="media/share-device-mode/cloud-administrator-provide-credentials.png " alt-text="Screenshot of the sign-in page in Authenticator app":::

1. Upon providing credentials, the device is successfully set in shared device mode.

   :::image type="content" source="media/share-device-mode/shared-device-mode-setup-successful.png " alt-text="Screenshot of a successful shared device mode setup in the Authenticator app":::

## Test shared device mode setup 

To see shared device mode in action, the [MSAL iOS Swift Microsoft Graph API](https://github.com/Azure-Samples/ms-identity-mobile-apple-swift-objc) code sample on GitHub includes an example of running a frontline worker app on an iOS device in shared device mode.
