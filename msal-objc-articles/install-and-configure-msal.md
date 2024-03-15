---
title: Install and configure MSAL for iOS/macOS
description: Learn how to install MSAL and configure your project to use the library
services: active-directory
author: Dickson-Mwendia
manager: CelesteDG

ms.service: identity-platform
ms.topic: conceptual
ms.workload: identity
ms.date: 09/22/2023
ms.author: dmwendia
ms.reviewer: oldalton, brianmel
ms.custom: aaddev
---

# Install MSAL and configure your project to use the library
 
## Installing  using CocoaPods

#### Installing CocoaPods
[CocoaPods](https://cocoapods.org) is a dependency manager for Swift and Objective-C Cocoa projects.
It can be installed as a ruby gem, by using the following command.
```
$ sudo gem install cocoapods
```
> Depending on your Ruby setup, `sudo` may or may not be needed.

#### Include MSAL in Podfile
Create a new `Podfile` or add to the existing podfile, include `pod 'MSAL'` under `target. 

An example Podfile:
```
target "your-target-here" do
    pod 'MSAL'
end
```

Note: if you're checking out a particular branch or tag of MSAL, you will need to add the :submodules => true flag to your podfile, so that CocoaPods finds MSAL dependencies

```
pod 'MSAL', :git => 'https://github.com/AzureAD/microsoft-authentication-library-for-objc', :branch => 'dev', :submodules => true
```

You then you can run either `pod install` (if it's a new PodFile) or `pod update` (if it's an existing PodFile) to get the latest version of MSAL. 

Subsequent calls to `pod update` will update to the latest released version of MSAL as well.

See [CocoaPods](https://guides.cocoapods.org/using/the-podfile.html) for more information on setting up a PodFile

## Installing using Carthage

[Carthage](https://github.com/Carthage/Carthage) is another popular dependency manager MSAL supports. 
The following is a sample using Carthage.

##### If you're building for iOS, tvOS, or watchOS

1. Install Carthage on your Mac using a download from their website or if using Homebrew `brew install carthage`.
1. You must create a `Cartfile` that lists the MSAL library for this project on Github. 

```
github "AzureAD/microsoft-authentication-library-for-objc" "dev"
```
Note: this will point Carthage to the dev branch, so you'll always get the latest official release. If you would like to use a particular version, you can do the following:

```
github "AzureAD/microsoft-authentication-library-for-objc" == <latest_released_version>
```

1. Run `carthage update`. This will fetch dependencies into a `Carthage/Checkouts` folder, then build the MSAL library.
1. On your application targets’ “General” settings tab, in the “Linked Frameworks and Libraries” section, drag and drop the `MSAL.framework` from the `Carthage/Build` folder on disk.
1. On your application targets’ “Build Phases” settings tab, click the “+” icon and choose “New Run Script Phase”. Create a Run Script in which you specify your shell (ex: `/bin/sh`), add the following contents to the script area below the shell:

  ```sh
  /usr/local/bin/carthage copy-frameworks
  ```

  and add the paths to the frameworks you want to use under “Input Files”, e.g.:

  ```
  $(SRCROOT)/Carthage/Build/iOS/MSAL.framework
  ```
  This script works around an [App Store submission bug](http://www.openradar.me/radar?id=6409498411401216) triggered by universal binaries and ensures that necessary bitcode-related files and dSYMs are copied when archiving.

With the debug information copied into the built products directory, Xcode will be able to symbolicate the stack trace whenever you stop at a breakpoint. This will also enable you to step through third-party code in the debugger.

When archiving your application for submission to the App Store or TestFlight, Xcode will also copy these files into the dSYMs subdirectory of your application’s `.xcarchive` bundle.


## Installing MSAL using Swift Packages

You can add `MSAL` as a [swift package dependency](https://developer.apple.com/documentation/swift_packages/distributing_binary_frameworks_as_swift_packages).
For MSAL version 1.1.14 and above, distribution of MSAL binary framework as a Swift package is available.

1. For your project in Xcode, click File → Swift Packages → Add Package Dependency...
2. Choose project to add dependency in
3. Enter : https://github.com/AzureAD/microsoft-authentication-library-for-objc as the package repository URL
4. Choose package options with :
    1. Rules → Branch : main (For latest MSAL release)
    2. Rules → Version → Exact : [release version >= 1.1.14] (For a particular release version)

For any issues, please check if there is an outstanding SPM/Xcode bug.
Workarounds for some bugs we encountered :

* If you have a plugin in your project you might encounter [CFBundleIdentifier collision. Each bundle must have a unique bundle identifier](https://github.com/AzureAD/microsoft-authentication-library-for-objc/issues/737#issuecomment-767311138) error. [Workaround](https://github.com/AzureAD/microsoft-authentication-library-for-objc/issues/737#issuecomment-767990771)
* While archiving, error : “IPA processing failed” UserInfo={NSLocalizedDescription=IPA processing failed}. [Workaround](https://github.com/AzureAD/microsoft-authentication-library-for-objc/issues/737#issuecomment-767990771)
* For a macOS app, “Command CodeSign failed with a nonzero exit code” error. [Workaround](https://github.com/AzureAD/microsoft-authentication-library-for-objc/issues/737#issuecomment-770056675)

## Install MSAL manually

1. Check out the repository using the following command.
```
git clone https://github.com/AzureAD/microsoft-authentication-library-for-objc.git --recursive
```

2. In your project, drag and add `microsoft-authentication-library-for-objc/MSAL/MSAL.xcodeproj`
3. In your project settings, for the target application, select `Build Phases` and add MSAL in `Target Dependencies`.

## Install MSAL using Git Submodule

If your project is managed in a git repository you can include MSAL as a git submodule. First check the GitHub Releases Page for the latest release tag. Replace <latest_release_tag> with that version.

* `git submodule add https://github.com/AzureAD/microsoft-authentication-library-for-objc msal`
* `cd msal`
* `git checkout tags/<latest_release_tag>`
* `git submodule update --init --recursive`
* `cd ..`
* `git add msal`
* `git commit -m "Use MSAL git submodule at <latest_release_tag>"`
* `git push`

## Configuring your project to use MSAL

### Adding MSAL to your project

1. Register your app in the [Azure portal](https://aka.ms/MobileAppReg)
2. Make sure you register a redirect URI for your application. It should be in the following format: 

 `msauth.$(PRODUCT_BUNDLE_IDENTIFIER)://auth`

3. Add a new keychain group to your project Capabilities. Keychain group should be `com.microsoft.adalcache` on iOS and `com.microsoft.identity.universalstorage` on macOS. 

See more information about [keychain groups](/entra/identity-platform/howto-v2-keychain-objc) and [Silent SSO for MSAL](/entra/identity-platform/single-sign-on-macos-ios).

#### iOS only steps:

1. Add your application's redirect URI scheme to your `Info.plist` file

```xml
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>msauth.$(PRODUCT_BUNDLE_IDENTIFIER)</string>
        </array>
    </dict>
</array>
```

2. Add `LSApplicationQueriesSchemes` to allow making call to Microsoft Authenticator if installed.

Note that “msauthv3” scheme is needed when compiling your app with Xcode 11 and later. 

```xml
<key>LSApplicationQueriesSchemes</key>
<array>
	<string>msauthv2</string>
	<string>msauthv3</string>
</array>
```
See more info about [configuring redirect uri for MSAL](/entra/identity-platform/reply-url)

3. To handle a callback, add the following to `appDelegate`:

#### Swift

```swift
func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {
        
	return MSALPublicClientApplication.handleMSALResponse(url, sourceApplication: options[UIApplication.OpenURLOptionsKey.sourceApplication] as? String)
}
```

#### Objective-C

```obj-c
- (BOOL)application:(UIApplication *)app
            openURL:(NSURL *)url
            options:(NSDictionary<UIApplicationOpenURLOptionsKey,id> *)options
{
    return [MSALPublicClientApplication handleMSALResponse:url 
                                         sourceApplication:options[UIApplicationOpenURLOptionsSourceApplicationKey]];
}
```

**Note, that if you adopted UISceneDelegate on iOS 13+**, MSAL callback needs to be placed into the appropriate delegate method of UISceneDelegate instead of AppDelegate. MSAL `handleMSALResponse:sourceApplication:` must be called only once for each URL. If you support both UISceneDelegate and UIApplicationDelegate for compatibility with older iOS, MSAL callback would need to be placed into both files.

#### Swift
```swift
func scene(_ scene: UIScene, openURLContexts URLContexts: Set<UIOpenURLContext>) {
        
        guard let urlContext = URLContexts.first else {
            return
        }
        
        let url = urlContext.url
        let sourceApp = urlContext.options.sourceApplication
        
        MSALPublicClientApplication.handleMSALResponse(url, sourceApplication: sourceApp)
    }
```

#### Objective-C
```objective-c
- (void)scene:(UIScene *)scene openURLContexts:(NSSet<UIOpenURLContext *> *)URLContexts
{
    UIOpenURLContext *context = URLContexts.anyObject;
    NSURL *url = context.URL;
    NSString *sourceApplication = context.options.sourceApplication;
    
    [MSALPublicClientApplication handleMSALResponse:url sourceApplication:sourceApplication];
}
```

#### macOS only steps:

1. Make sure your application is signed with a valid development certificate. While MSAL will still work in the unsigned mode, it will behave differently around cache persistence.

### Microsoft Enterprise SSO plug-in for Apple devices

Microsoft has recently released a new plug-in that uses the newly announced Apple feature called [Enterprise Single Sign-On](https://developer.apple.com/documentation/authenticationservices). Microsoft Enterprise SSO plug-in for Apple devices offers the following benefits: 

* Comes delivered in Microsoft Authenticator app automatically and can be enabled by any MDM.
* Provides seamless SSO for Active Directory joined accounts across all applications that support Apple's Enterprise Single Sign-On feature.
* COMING SOON: Provides seamless SSO across Safari browsers and applications on the device.

MSAL 1.1.0 and above will use Microsoft Enterprise SSO plug-in automatically instead of the Microsoft Authenticator app when it is active on the device. To use Microsoft Enterprise SSO plug-in in your tenant, you need to enable it in your MDM profile. 

See [more information](/entra/identity-platform/apple-sso-plugin) about configuring Microsoft Enterprise SSO plug-in for your device [here](/entra/identity-platform/apple-sso-plugin)

### Single Account Mode

If your app needs to support just one signed-in user at a time, MSAL provides a simple way to read the signed in account. This API must be also used when you are building an application to run on devices that are configured as shared devices - meaning that a single corporate device is shared between multiple employees. Employees can sign in to their devices and access customer information quickly. When they are finished with their shift or task, they will be able to sign-out of all apps on the shared device.

Here is a code snippet that shows how you can retrieve current account. You must call API every time when your app comes to foreground or before performing a sensitive operation to detect any signed-in account changes. 

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

### Multiple Accounts Mode

MSAL also provides a public API to query multiple accounts, granted that they exist in the MSAL cache.

1) Make sure the umbrella header MSAL-umbrella.h is imported (just MSAL for Swift)

2) Create config, then use it to initialize an application object 

3) Also initialize `MSALAccountEnumerationParameters`` object with the account identifier. Each MSALAccount object has a parameter called “identifier”, which represents the unique account identifier associated with the given MSALAccount object. We recommend using it as the primary search criterion. 

4) Then invoke the API "accountsFromDeviceForParameters" from the application object using the enumeration parameter. If you have multiple accounts in MSAL cache, it will return an array containing MSALAccounts that have the account identifier you specified in the previous step. 

5) Once the MSAL account is retrieved, invoke acquire token silent operation

#### Swift

```swift
#import MSAL //Make sure to import MSAL  

let config = MSALPublicClientApplicationConfig(clientId:clientId
                                           	redirectUri:redirectUri
                                            	authority:authority)
guard let application = MSALPublicClientApplication(configuration: config) else { return }

let accountIdentifier = "9f4880d8-80ba-4c40-97bc-f7a23c703084.f645ad92-e38d-4d1a-b510-d1b09a74a8ca"
let parameters = MSALAccountEnumerationParameters(identifier:accountIdentifier)

var scopeArr = ["https://graph.microsoft.com/.default"]

if #available(macOS 10.15, *)
{
	 application.accountsFromDeviceForParameters(with: parameters, completionBlock:{(accounts, error) in
         if let error = error 
         {
            //Handle error
         }
         
         guard let accountObjs = accounts else {return}
         
         let tokenParameters = MSALSilentTokenParameters(scopes:scopeArr, account: accountObjs[0]);
                                                                                                   
         application.acquireTokenSilentWithParameters(with: tokenParameters, completionBlock:{(result, error) in 
                     if let error = error
                     {
                         //handle error
                     }
                                       
                     guard let resp = result else {return} //process result
                                                                                             
         })                                                               
                                                                                                                                                             
   })
  
}
```


#### Objective-C

```objective-c
//import other key libraries  
#import "MSAL-umbrella.h" //Make sure to import umbrella file 

    MSALPublicClientApplicationConfig *config = [[MSALPublicClientApplicationConfig alloc] initWithClientId:clientId
     redirectUri:redirectUri
       authority:authority];

    MSALPublicClientApplication *application = [[MSALPublicClientApplication alloc] initWithConfiguration:config error:&error];
    MSALAccountEnumerationParameters *parameters = [[MSALAccountEnumerationParameters alloc] initWithIdentifier:@"9f4880d8-80ba-4c40-97bc-f7a23c703084.f645ad92-e38d-4d1a-b510-d1b09a74a8ca"]; //init with account identifier

    NSArray<NSString *> *scopeArr = [[NSArray alloc] initWithObjects: @"https://graph.microsoft.com/.default",nil]; //define scope

    if (@available(macOS 10.15, *)) //Currently, this public API requires macOs version 10.15 or greater.
    {
        [application accountsFromDeviceForParameters:parameters
                                     completionBlock:^(NSArray<MSALAccount *> * _Nullable accounts, __unused NSError * _Nullable error)
        {
            if (error)
            {
              //Log error & return 
            }
          
            if (accounts)
            {
                NSLog(@"hi there");
                MSALSilentTokenParameters *tokenParameters = [[MSALSilentTokenParameters alloc] initWithScopes:scopeArr account:accounts[0]];

                [application acquireTokenSilentWithParameters:tokenParameters
                                completionBlock:^(MSALResult * _Nullable result, NSError * _Nullable error)
                 {
                    if (error)
                    {
                        //Log Error & return 
                    }
                    if (result)
                    {
                        //process result
                    }
                }
                 ];
            }
     
        }];
    }
```

### Detect shared device mode

Use following code to read current device configuration, including whether device is configured as shared:

#### Swift

```swift
application.getDeviceInformation(with: nil, completionBlock: { (deviceInformation, error) in
                
	guard let deviceInfo = deviceInformation else {
		return
	}
                
	let isSharedDevice = deviceInfo.deviceMode == .shared
	// Change your app UX if needed
})
```

#### Objective-C

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

### Implement signout

To signout account from your app, call MSAL's signout API. You can also optionally sign out from the browser. When MSAL is running on a shared device, signout API will signout globally from all apps on user's device.

#### Swift

```swift
let account = .... /* account retrieved above */

let signoutParameters = MSALSignoutParameters(webviewParameters: self.webViewParameters!)
signoutParameters.signoutFromBrowser = false
            
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
signoutParameters.signoutFromBrowser = NO;
        
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

