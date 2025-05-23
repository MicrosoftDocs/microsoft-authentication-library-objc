---
title: Microsoft Authentication Library for iOS and macOS
description: Learn about the Microsoft Authentication Library for iOS and macOS
author: Dickson-Mwendia
manager: CelesteDG
ms.author: dmwendia
ms.date: 03/26/2024
ms.service: msal
ms.subservice: msal-ios-mac
ms.reviewer: oldalton
ms.topic: conceptual
ms.custom: aaddev
#Customer intent: 

---

# Microsoft Authentication Library for iOS and macOS

The Microsoft Authentication Library (MSAL) for iOS and macOS is an auth SDK that can be used to seamlessly integrate authentication into your apps using industry standard OAuth2 and OpenID Connect. It allows you to sign in users or apps with Microsoft identities. These identities include Microsoft Entra ID work and school accounts, personal Microsoft accounts, social accounts, and customer accounts. 

Using MSAL for iOS and macOS, you can acquire security tokens from the Microsoft identity platform to authenticate users and access secure web APIs for their applications. The library supports multiple authentication scenarios, such as single sign-on (SSO), Conditional Access, and brokered authentication. 

#### Native authentication support in MSAL

MSAL iOS provides native authentication APIs that allow applications to implement a native experience with end-to-end customizable flows in their mobile applications. With native authentication, users are guided through a rich, native, mobile-first sign-up and sign-in journey without leaving the app. The native authentication feature is only available for mobile apps on [External ID for customers](/entra/external-id/customers/concept-native-authentication). macOS is not supported. It is recommended to always use the most up-to-date version of the SDK.

## Getting started with MSAL for iOS and macOS conceptual documentation

The MSAL documentation covers common patterns, error handling and debugging best practices, extra library functionality (such as logging, telemetry), and any active bugs or common issues with known mitigations. If you're looking for more help getting started with Microsoft Entra ID, Microsoft Accounts, or Azure AD B2C, check out the [Microsoft identity platform docs](https://aka.ms/aaddev). If you're looking for more info about the Microsoft Graph API, check out the [Microsoft Graph docs](https://graph.microsoft.io). To effectively use MSAL iOS and macOS to protect your applications, it's important to familiarize yourself with the following concepts:


1. Learn the differences between MSAL for iOS and macOS
1. [Register your app](/entra/identity-platform/quickstart-register-app) with Microsoft Entra
1. [Install MSAL for iOS and macOS and configure your project](install-and-configure-msal.md) to use the library
1. [Acquire tokens](acquire-tokens.md) to access a protected API


To use MSAL iOS and macOS in your application, you need to register your application in the Microsoft Entra Admin center and configure your project. Since the SDK supports both browser-delegated and native authentication experiences, follow the steps in the one of these quickstarts based on your scenario.

* For browser-delegated authentication scenarios, refer to the quickstart, [Sign in users and call Microsoft Graph from an iOS or macOS app](/entra/identity-platform/quickstart-mobile-app-ios-sign-in).

* For native authentication scenarios on iOS apps, refer to the Microsoft Entra External ID sample guide, [Run iOS sample app](/entra/external-id/customers/how-to-run-native-authentication-sample-ios-app).

## Supported Versions

**iOS** - MSAL supports iOS 14 and above.

**macOS** - MSAL supports macOS (OSX) 10.13 and above.


## Key differences between the Microsoft Authentication Library for iOS and macOS

This section explains the differences in functionality between the Microsoft Authentication Library (MSAL) for iOS and macOS.

> [!NOTE]
> On the Mac, MSAL only supports macOS apps.

## General differences

MSAL for macOS is a subset of the functionality available for iOS.

MSAL for macOS doesn't support:

- different browser types such as `ASWebAuthenticationSession`, `SFAuthenticationSession`, `SFSafariViewController`.
- brokered authentication through Microsoft Authenticator app is not supported for macOS.

Keychain sharing between apps from the same publisher is more limited on macOS 10.14 and earlier. Use [access control lists](https://developer.apple.com/documentation/security/keychain_services/access_control_lists?language=objc) to specify the paths to the apps that should share the keychain. User may see additional keychain prompts.

On macOS 10.15+, MSAL's behavior is the same between iOS and macOS. MSAL uses [keychain access groups](https://developer.apple.com/documentation/security/keychain_services/keychain_items/sharing_access_to_keychain_items_among_a_collection_of_apps?language=objc) for keychain sharing.

### Conditional Access authentication

For Conditional Access scenarios, there will be fewer user prompts when you use MSAL for iOS. This is because iOS uses the broker app (Microsoft Authenticator) which negates the need to prompt the user in some cases.

### Project setup 

**macOS**

- When you set up your project on macOS, ensure that your application is signed with a valid development or production certificate. MSAL still works in the unsigned mode, but it will behave differently with regards to cache persistence. The app should only be run unsigned for debugging purposes. If you distribute the app unsigned, it will:
1. On 10.14 and earlier, MSAL will prompt the user for a keychain password every time they restart the app.
2. On 10.15+, MSAL will prompt user for credentials for every token acquisition.

- macOS apps don't need to implement the AppDelegate call.

**iOS**

- There are additional steps to set up your project to support authentication broker flow. The steps are called out in the tutorial.
- iOS projects need to register custom schemes in the info.plist. This isn't required on macOS.


## Migration from Azure Active Directory Authentication Library (ADAL)

The Azure Active Directory Authentication Library (ADAL) for Objective-C has been deprecated effective **June 30, 2023**.. If you or your organization are using the Azure Active Directory Authentication Library (ADAL), you should [migrate to MSAL](migrate-objc-adal-msal.md) to avoid putting your app's security at risk.


## Samples

See [our comprehensive sample list](/entra/identity-platform/sample-v2-code?tabs=apptype#mobile).

## Get help with the SDK

If you have a question about MSAL SDK, found an error in the documentation or need a recommendation on, please create a [Github issue](https://github.com/AzureAD/microsoft-authentication-library-for-objc/issues) with the details.