---
title: Support for iOS 13 in MSAL
description: Learn ABOUT support for iOS 13 in MSAL
author: Dickson-Mwendia
manager: CelesteDG
ms.author: dmwendia
ms.date: 02/19/2024
ms.service: msal
ms.subservice: msal-ios-mac
ms.reviewer: oldalton
ms.topic: how-to
ms.custom: aaddev
#Customer intent: 

---

# Support for iOS 13 in MSAL

If your app requires conditional access or certificate authentication support, you must set up your MSAL to be able to talk to the Azure Authenticator app.

MSAL is then responsible for handling requests and responses between your application and the Azure Authenticator app.

However, on iOS 13, Apple made a breaking API change, and removed application's ability to read source application when receiving a response from an external application through custom URL schemes. See notes from Apple [here](https://developer.apple.com/documentation/uikit/uiapplicationopenurloptionssourceapplicationkey?language=objc). 

If the request originated from another app belonging to your team, UIKit sets the value of this key to the ID of that app. If the team identifier of the originating app is different than the team identifier of the current app, the value of the key is nil.

This is a breaking change for MSAL, because it relied on the `UIApplicationOpenURLOptionsSourceApplicationKey` to verify communication between MSAL and the Azure Authenticator app. 

Additionally, on iOS 13 developer is required to provide a presentation controller when using ASWebAuthenticationSession.

In order to mitigate these changes, we released a new MSAL version with iOS 13 support:
- [MSAL 0.7.0](https://github.com/AzureAD/microsoft-authentication-library-for-objc/releases/tag/0.7.0)

### Your app is impacted if:

1. Your app is leveraging iOS broker, AND you're building with Xcode 11, OR
2. You're using ASWebAuthenticationSession, AND you're building with Xcode 11.

In those cases you need to use latest MSAL releases to be able to complete authentication successfully.

### Your app is NOT impacted if:

1. Your app is not using iOS broker, OR
2. Your app is being built with Xcode 11, OR
3. Your app is distributed by Microsoft (signed by Microsoft developer distribution profile), OR
4. You're not using ASWebAuthenticationSession.

### Additional considerations:

1. When using latest MSAL SDKs, you need to ensure that you have the latest Authenticator app installed. Authenticator app with a version 6.3.19 or later is supported. 

2. When updating to this release, make sure you update your LSApplicationQueriesSchemes in the Info.plist. 
New value should be:

```
<key>LSApplicationQueriesSchemes</key>
<array>
     <string>msauthv2</string>
     <string>msauthv3</string>
</array>
```

This is necessary to detect the presence of the latest Authenticator app on device that supports iOS 13. 

Please open [a Github issue](https://github.com/AzureAD/microsoft-authentication-library-for-objc/issues) if you have additional questions or seeing any issues. 
