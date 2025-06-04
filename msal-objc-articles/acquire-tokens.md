---
title: Acquire tokens in MSAL for iOS/macOS
description: Learn how to acquire tokens using MSAL to authenticate users in iOS/macOS applications.
author: Dickson-Mwendia
manager: CelesteDG
ms.author: dmwendia
ms.date: 02/19/2024
ms.service: msal
ms.subservice: msal-ios-mac
ms.reviewer: oldalton
ms.topic: conceptual
#Customer intent: 

---

# Using MSAL to acquire tokens

## Creating an Application Object

Use the client ID from your app listing when initializing your MSALPublicClientApplication object:

### Swift

```swift
let config = MSALPublicClientApplicationConfig(clientId: "<your-client-id-here>")
let application = try? MSALPublicClientApplication(configuration: config) 
```

### Objective-C

```obj-c
NSError *msalError = nil;
    
MSALPublicClientApplicationConfig *config = [[MSALPublicClientApplicationConfig alloc] initWithClientId:@"<your-client-id-here>"];
MSALPublicClientApplication *application = [[MSALPublicClientApplication alloc] initWithConfiguration:config error:&msalError];
    
```

## Acquiring a token interactively

#### Swift

```swift
#if os(iOS)
	let viewController = ... // Pass a reference to the view controller that should be used when getting a token interactively
	let webviewParameters = MSALWebviewParameters(authPresentationViewController: viewController)
#else
	let webviewParameters = MSALWebviewParameters()
#endif
let interactiveParameters = MSALInteractiveTokenParameters(scopes: scopes, webviewParameters: webviewParameters)
application.acquireToken(with: interactiveParameters, completionBlock: { (result, error) in
                
	guard let authResult = result, error == nil else {
		print(error!.localizedDescription)
		return
	}
                
	// Get access token from result
	let accessToken = authResult.accessToken
                
	// You'll want to get the account identifier to retrieve and reuse the account for later acquireToken calls
	let accountIdentifier = authResult.account.identifier
})
```

### Objective-C

```obj-c
#if TARGET_OS_IPHONE
    UIViewController *viewController = ...; // Pass a reference to the view controller that should be used when getting a token interactively
    MSALWebviewParameters *webParameters = [[MSALWebviewParameters alloc] initWithAuthPresentationViewController:viewController];
#else
    MSALWebviewParameters *webParameters = [MSALWebviewParameters new];
#endif 

MSALInteractiveTokenParameters *interactiveParams = [[MSALInteractiveTokenParameters alloc] initWithScopes:scopes webviewParameters:webParameters];
[application acquireTokenWithParameters:interactiveParams completionBlock:^(MSALResult *result, NSError *error) {
	if (!error)	
	{
		// You'll want to get the account identifier to retrieve and reuse the account
		// for later acquireToken calls
		NSString *accountIdentifier = result.account.identifier;
            
		NSString *accessToken = result.accessToken;
	}
  	else
	{
		// Check the error
	}
}];
```

>[!NOTE]
> Our library uses the ASWebAuthenticationSession for authentication on iOS 12 by default. See more information about [default values, and support for other iOS versions](/entra/identity-platform/customize-webviews).

## Acquiring a token silently

### Swift

```swift
guard let account = try? application.account(forIdentifier: accountIdentifier) else { return }
let silentParameters = MSALSilentTokenParameters(scopes: scopes, account: account)
application.acquireTokenSilent(with: silentParameters) { (result, error) in
            
	guard let authResult = result, error == nil else {
                
	let nsError = error! as NSError
                
		if (nsError.domain == MSALErrorDomain &&
			nsError.code == MSALError.interactionRequired.rawValue) {
                    
			// Interactive auth will be required
			return
		}
		return
	}
            
	// Get access token from result
	let accessToken = authResult.accessToken
}
```

### Objective-C

```objective-c
NSError *error = nil;
MSALAccount *account = [application accountForIdentifier:accountIdentifier error:&error];
if (!account)
{
    // handle error
    return;
}
    
MSALSilentTokenParameters *silentParams = [[MSALSilentTokenParameters alloc] initWithScopes:scopes account:account];
[application acquireTokenSilentWithParameters:silentParams completionBlock:^(MSALResult *result, NSError *error) {
    if (!error)
    {
        NSString *accessToken = result.accessToken;
    }
    else
    {
        // Check the error
        if ([error.domain isEqual:MSALErrorDomain] && error.code == MSALErrorInteractionRequired)
        {
            // Interactive auth will be required
        }
            
        // Other errors may require trying again later, or reporting authentication problems to the user
    }
}];
```

## Responding to an interaction required error

Occasionally user interaction will be required to get a new access token, when this occurs you will receive a `MSALErrorInteractionRequired` error when trying to silently acquire a new token. In those cases call `acquireToken:` with the same account and scopes as the failing `acquireTokenSilent:` call. It is recommended to display a status message to the user in an unobtrusive way before invoking interactive `acquireToken:` call.

