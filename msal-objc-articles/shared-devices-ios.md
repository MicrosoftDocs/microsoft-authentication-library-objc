---
title: Shared device mode for iOS devices
description: Learn how to enable shared device mode to allow frontline workers to share an iOS device
services: active-directory
author: henrymbuguakiarie
manager: CelesteDG

ms.service: msal
ms.subservice: msal-ios-mac
ms.topic: tutorial
ms.date: 06/14/2024
ms.author: henrymbugua
ms.reviewer: oldalton, negoe, akgoel23
ms.custom: aaddev
---

# Modify your iOS or iPadOS application to support shared device mode

In this tutorial, you learn how to modify an iOS or iPadOS application to support Shared Device Mode (SDM). SDM is a Microsoft Entra ID feature enabling organizations to configure an iOS, iPadOS, or Android device for easy sharing among multiple employees, a common practice in frontline worker settings.

In this tutorial, you:

> [!div class="checklist"]
> * Add support for single account mode.
> * Configure your app to use SDM.
> * Detect shared device mode.
> * Identify if the signed-in user has changed.

## Prerequisites

- [Tutorial: Sign in users and call Microsoft Graph from an iOS or macOS app](/entra/identity-platform/tutorial-v2-ios)

## Detect shared device mode

Detecting shared device mode is important for your application. Many applications require a change in their user experience (UX) when the application is used on a shared device. For example, your application might have a "Sign-Up" feature, which isn't appropriate for a frontline worker because they likely already have an account. You may also want to add extra security to your application's handling of data if it's in shared device mode.

Use the `getDeviceInformationWithParameters:completionBlock:` API in the `MSALPublicClientApplication` to determine if an app is running on a device in shared device mode.

The following code snippets show examples of using the `getDeviceInformationWithParameters:completionBlock:` API.

### Swift

```swift
application.getDeviceInformation(with: nil, completionBlock: { (deviceInformation, error) in

    guard let deviceInfo = deviceInformation else {
        return
    }

    let isSharedDevice = deviceInfo.deviceMode == .shared
    // Change your app UX if needed
})
```

### Objective-C

```objective-c
[application getDeviceInformationWithParameters:nil
                                completionBlock:^(MSALDeviceInformation * _Nullable deviceInformation, NSError * _Nullable error)
{
    if (!deviceInformation)
    {
        return;
    }

    BOOL isSharedDevice = deviceInformation.deviceMode == MSALDeviceModeShared;
    // Change your app UX if needed
}];
```

## Get the signed-in user and determine if a user has changed on the device

Another important part of supporting shared device mode is determining the state of the user on the device and clearing application data if a user has changed or if there's no user at all on the device. You're responsible for ensuring data isn't leaked to another user.

You can use `getCurrentAccountWithParameters:completionBlock:` API to query the currently signed-in account on the device.

#### Swift

```swift
let msalParameters = MSALParameters()
msalParameters.completionBlockQueue = DispatchQueue.main

application.getCurrentAccount(with: msalParameters, completionBlock: { (currentAccount, previousAccount, error) in

    // currentAccount is the currently signed in account
    // previousAccount is the previously signed in account if any
})
```

#### Objective-C

```objective-c
MSALParameters *parameters = [MSALParameters new];
parameters.completionBlockQueue = dispatch_get_main_queue();

[application getCurrentAccountWithParameters:parameters
                             completionBlock:^(MSALAccount * _Nullable account, MSALAccount * _Nullable previousAccount, NSError * _Nullable error)
{
    // currentAccount is the currently signed in account
    // previousAccount is the previously signed in account if any
}];
```

### Globally sign in a user

When a device is configured as a shared device, your application can call the `acquireTokenWithParameters:completionBlock:` API to sign in the account. The account will be available globally for all eligible apps on the device after the first app signs in the account.

#### Objective-C

```objective-c
MSALInteractiveTokenParameters *parameters = [[MSALInteractiveTokenParameters alloc] initWithScopes:@[@"api://myapi/scope"] webviewParameters:[self msalTestWebViewParameters]];

parameters.loginHint = self.loginHintTextField.text;

[application acquireTokenWithParameters:parameters completionBlock:completionBlock];
```

### Globally sign out a user

The following code removes the signed-in account and clears cached tokens from not only the app, but also from the device that's in shared device mode. It doesn't, however, clear the _data_ from your application. You must clear the data from your application, and clear any cached data your application may be displaying to the user.

#### Swift

```swift
let account = .... /* account retrieved above */

let signoutParameters = MSALSignoutParameters(webviewParameters: self.webViewParamaters!)
signoutParameters.signoutFromBrowser = true // To trigger a browser signout in Safari.

application.signout(with: account, signoutParameters: signoutParameters, completionBlock: {(success, error) in
    if let error = error {

        // Signout failed

        return

    }

    // Sign out completed successfully

})
```

#### Objective-C

```objective-c
MSALAccount *account = ... /* account retrieved above */;

MSALSignoutParameters *signoutParameters = [[MSALSignoutParameters alloc] initWithWebviewParameters:webViewParameters];

signoutParameters.signoutFromBrowser = YES; // To trigger a browser signout in Safari.

[application signoutWithAccount:account signoutParameters:signoutParameters completionBlock:^(BOOL success, NSError * _Nullable error)

{

    if (!success)

    {

        // Signout failed

        return;

    }

    // Sign out completed successfully

}];
```

The [Microsoft Enterprise SSO plug-in for Apple devices](/entra/identity-platform/apple-sso-plugin) clears state only for applications. It doesn't clear state on the Safari browser. You can use the optional `signoutFromBrowser` property shown in code snippets to trigger a browser sign out in Safari. This causes the browser to briefly launch on the device.

### Receive broadcast to detect global sign out initiated from other applications

To receive the account change broadcast, you need to register a broadcast receiver. When an account change broadcast is received, immediately [get the signed in user and determine if a user has changed on the device](#get-the-signed-in-user-and-determine-if-a-user-has-changed-on-the-device). If a change is detected, initiate data cleanup for previously signed-in account. It's recommended to properly stop any operations and do data cleanup.

The following code snippet shows how you could register a broadcast receiver.

```objectivec
NSString *const MSAL_SHARED_MODE_CURRENT_ACCOUNT_CHANGED_NOTIFICATION_KEY = @"SHARED_MODE_CURRENT_ACCOUNT_CHANGED";

- (void) registerDarwinNotificationListener 

{ 

   CFNotificationCenterRef center =

   CFNotificationCenterGetDarwinNotifyCenter(); 

   CFNotificationCenterAddObserver(center, nil,

   sharedModeAccountChangedCallback,

   (CFStringRef)MSAL_SHARED_MODE_CURRENT_ACCOUNT_CHANGED_NOTIFICATION_KEY, 

   nil, CFNotificationSuspensionBehaviorDeliverImmediately); 

} 

// CFNotificationCallbacks used specifically for Darwin notifications leave userInfo unused 

void sharedModeAccountChangedCallback(CFNotificationCenterRef center, void * observer, CFStringRef name, void const * object, __unused CFDictionaryRef userInfo) 

{ 

    // Invoke account cleanup logic here 

} 
```

For more information about the available options for `CFNotificationAddObserver` or to see the corresponding method signatures in Swift, see:

- [CFNotificationAddObserver](https://developer.apple.com/documentation/corefoundation/1543316-cfnotificationcenteraddobserver?language=objc)
- [CFNotificationCallback](https://developer.apple.com/documentation/corefoundation/cfnotificationcallback?language=objc)

For iOS, your app requires a background permission to remain active in the background and listen to Darwin notifications. The background capability must be added to support a different background operation – your app may be subject to rejection from the Apple App Store if it has a background capability only to listen for Darwin notifications. If your app is already configured to complete background operations, you can add the listener as part of that operation. For more information about iOS background capabilities, see [Configuring background execution modes](https://developer.apple.com/documentation/xcode/configuring-background-execution-modes)

## Microsoft applications that support shared device mode

These Microsoft applications support Microsoft Entra shared device mode:

- [Microsoft Teams](/microsoftteams/platform/)
- [Microsoft Viva Engage](/viva/engage/overview) (previously [Yammer](/viva/engage/overview))
- [Outlook](/mem/intune/apps/app-configuration-policies-outlook) (in Private Preview)
- [Microsoft Power Apps](/power-apps/)
- [Microsoft 365](/microsoft-365-apps/mac/configure-shared-device-mode)
- [Microsoft Power BI Mobile](/power-bi/consumer/mobile/mobile-app-shared-device-mode)
- [Microsoft Edge](/microsoft-edge/)


> [!IMPORTANT]
> Public preview is provided without a service-level agreement and isn't recommended for production workloads. Some features might be unsupported or have constrained capabilities. For more information, see [Universal License Terms for Online Services](https://www.microsoft.com/licensing/terms/product/ForOnlineServices/all).

## Next steps

To see shared device mode in action, the following code sample on GitHub includes an example of running a frontline worker app on an iOS device in shared device mode:

[MSAL iOS Swift Microsoft Graph API Sample](https://github.com/Azure-Samples/ms-identity-mobile-apple-swift-objc)
