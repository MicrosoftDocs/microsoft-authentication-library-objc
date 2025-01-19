---
title: MSALPublicClientApplication

---

# MSALPublicClientApplication





## Public Functions

|                | Name           |
| -------------- | -------------- |
| virtual nonnull NSOrderedSet * | **[defaultOIDCScopes](Classes/class_m_s_a_l_public_client_application.md#function-defaultoidcscopes)**() |
| virtual BOOL | **[shouldExcludeValidationForAuthority:](Classes/class_m_s_a_l_public_client_application.md#function-shouldexcludevalidationforauthority:)**(nonnull MSIDAuthority * authority) |

## Public Properties

|                | Name           |
| -------------- | -------------- |
| MSIDDefaultTokenCacheAccessor * | **[tokenCache](Classes/class_m_s_a_l_public_client_application.md#property-tokencache)**  |
| MSIDAccountMetadataCacheAccessor * | **[accountMetadataCache](Classes/class_m_s_a_l_public_client_application.md#property-accountmetadatacache)**  |
| MSALOauth2Provider * | **[msalOauth2Provider](Classes/class_m_s_a_l_public_client_application.md#property-msaloauth2provider)**  |
| MSALExternalAccountHandler * | **[externalAccountHandler](Classes/class_m_s_a_l_public_client_application.md#property-externalaccounthandler)**  |
| MSALPublicClientApplicationConfig * | **[internalConfig](Classes/class_m_s_a_l_public_client_application.md#property-internalconfig)**  |
| MSIDExternalAADCacheSeeder * | **[externalCacheSeeder](Classes/class_m_s_a_l_public_client_application.md#property-externalcacheseeder)**  |
| MSIDCacheConfig * | **[msidCacheConfig](Classes/class_m_s_a_l_public_client_application.md#property-msidcacheconfig)**  |
| MSIDDevicePopManager * | **[popManager](Classes/class_m_s_a_l_public_client_application.md#property-popmanager)**  |
| MSIDAssymetricKeyLookupAttributes * | **[keyPairAttributes](Classes/class_m_s_a_l_public_client_application.md#property-keypairattributes)**  |

## Protected Attributes

|                | Name           |
| -------------- | -------------- |
| WKWebView * | **[_customWebview](Classes/class_m_s_a_l_public_client_application.md#variable--customwebview)**  |
| NSString * | **[_defaultKeychainGroup](Classes/class_m_s_a_l_public_client_application.md#variable--defaultkeychaingroup)**  |

## Public Functions Documentation

### function defaultOIDCScopes

```objective-c
static virtual nonnull NSOrderedSet * defaultOIDCScopes()
```


### function shouldExcludeValidationForAuthority:

```objective-c
virtual BOOL shouldExcludeValidationForAuthority:(
    nonnull MSIDAuthority * authority
)
```


## Public Property Documentation

### property tokenCache

```objective-c
MSIDDefaultTokenCacheAccessor * tokenCache;
```


### property accountMetadataCache

```objective-c
MSIDAccountMetadataCacheAccessor * accountMetadataCache;
```


### property msalOauth2Provider

```objective-c
MSALOauth2Provider * msalOauth2Provider;
```


### property externalAccountHandler

```objective-c
MSALExternalAccountHandler * externalAccountHandler;
```


### property internalConfig

```objective-c
MSALPublicClientApplicationConfig * internalConfig;
```


### property externalCacheSeeder

```objective-c
MSIDExternalAADCacheSeeder * externalCacheSeeder;
```


### property msidCacheConfig

```objective-c
MSIDCacheConfig * msidCacheConfig;
```


### property popManager

```objective-c
MSIDDevicePopManager * popManager;
```


### property keyPairAttributes

```objective-c
MSIDAssymetricKeyLookupAttributes * keyPairAttributes;
```


## Protected Attributes Documentation

### variable _customWebview

```objective-c
WKWebView * _customWebview;
```


### variable _defaultKeychainGroup

```objective-c
NSString * _defaultKeychainGroup;
```


-------------------------------

Updated on 2023-10-18 at 16:35:54 -0700