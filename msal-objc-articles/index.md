# Microsoft Authentication Library for iOS and macOS



The MSAL library for iOS and macOS gives your app the ability to begin using the [Microsoft Identity platform](https://aka.ms/aaddev) by supporting [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) and [Microsoft Accounts](https://account.microsoft.com) in a converged experienceThe library also supports [Azure AD B2C](https://azure.microsoft.com/services/active-directory-b2c/) for those using our hosted identity management service.

The Objective-C Microsoft Authentication Library (MSAL) is an Auth SDK that can be used to seamlessly integrate authentication into your app  using industry standard OAuth2 and OpenID Connect. 

This platform allows you to easily target several identities including Azure AD (Work and School accounts), Microsoft account (Outlook.com, hotmail.com, and several others), or Azure AD B2C (Social and Local accounts).  Your app will then have access to the entire set of Microsoft cloud APIs including the Microsoft Graph, Office 365, Azure, and any API you want to protect.  

## Getting started with MSAL for iOS and macOS conceptual documentation

The MSAL documentation covers common patterns, error handling and debugging best practices, extra library functionality (such as logging, telemetry), and any active bugs or common issues with known mitigations. If you're looking for more help getting started with Azure AD, Microsoft Accounts, or Azure AD B2C, check out the full [Microsoft identity platform docs](https://aka.ms/aaddev). If you're looking for more info about the Microsoft Graph API, check out the [Microsoft Graph docs](https://graph.microsoft.io).

1. Learn the differences between MSAL for iOS and macOS(./getting-started/scenarios.md).
1. You will need to [register your app](/azure/active-directory/develop/quickstart-register-app) with Microsoft Entra
1. Learn how to install MSAL
1. Learn how to configure MSAL [acquiring tokens](acquiring-tokens/overview.md) to access a protected API.
1. Learn how to acquire  tokens


## Supported Versions

**iOS** - MSAL supports iOS 14 and above.

**macOS** - MSAL supports macOS (OSX) 10.13 and above.

## Migration from Azure Active Directory Authentication Library (ADAL)

Microsoft Authentication Library (MSAL) for iOS and macOS is the supported library that can be used for authentication token acquisition. If you or your organization are using the Azure Active Directory Authentication Library (ADAL), you should [migrate to MSAL](/azure/active-directory/develop/msal-migration). ADAL was deprecated on **June 30, 2023**.

## Samples

See [our comprehensive sample list](/azure/active-directory/develop/active-directory-v2-code-samples).

## Get help with the SDK

If you have a question about MSAL SDK, found an error in the documentation or need a recommendation on, please create a [Github issue](https://github.com/AzureAD/microsoft-authentication-library-for-objc/issues) with the details.