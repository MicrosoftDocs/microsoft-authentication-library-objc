---
title: MSALErrorConverter

---

# MSALErrorConverter





Inherits from NSObject

## Public Functions

|                | Name           |
| -------------- | -------------- |
| virtual NSError * | **[msalErrorFromMsidError:](Classes/interface_m_s_a_l_error_converter.md#function-msalerrorfrommsiderror:)**(NSError * msidError) |
| virtual NSError * | **[msalErrorFromMsidError:classifyErrors:msalOauth2Provider:](Classes/interface_m_s_a_l_error_converter.md#function-msalerrorfrommsiderror:classifyerrors:msaloauth2provider:)**(NSError * msidError, BOOL shouldClassifyErrors, MSALOauth2Provider * oauth2Provider) |
| virtual NSError * | **[msalErrorFromMsidError:classifyErrors:msalOauth2Provider:correlationId:authScheme:popManager:](Classes/interface_m_s_a_l_error_converter.md#function-msalerrorfrommsiderror:classifyerrors:msaloauth2provider:correlationid:authscheme:popmanager:)**(NSError * msidError, BOOL shouldClassifyErrors, MSALOauth2Provider * oauth2Provider, NSUUID * correlationId, id< MSALAuthenticationSchemeProtocol > authScheme, MSIDDevicePopManager * popManager) |
| virtual NSError * | **[errorWithDomain:code:errorDescription:oauthError:subError:underlyingError:correlationId:userInfo:classifyErrors:msalOauth2Provider:authScheme:popManager:](Classes/interface_m_s_a_l_error_converter.md#function-errorwithdomain:code:errordescription:oautherror:suberror:underlyingerror:correlationid:userinfo:classifyerrors:msaloauth2provider:authscheme:popmanager:)**(NSString * domain, NSInteger code, NSString * errorDescription, NSString * oauthError, NSString * subError, NSError * underlyingError, NSUUID * correlationId, NSDictionary * userInfo, BOOL shouldClassifyErrors, MSALOauth2Provider * oauth2Provider, id< MSALAuthenticationSchemeProtocol > authScheme, MSIDDevicePopManager * popManager) |

## Public Functions Documentation

### function msalErrorFromMsidError:

```objective-c
static virtual NSError * msalErrorFromMsidError:(
    NSError * msidError
)
```


### function msalErrorFromMsidError:classifyErrors:msalOauth2Provider:

```objective-c
static virtual NSError * msalErrorFromMsidError:classifyErrors:msalOauth2Provider:(
    NSError * msidError,
    BOOL shouldClassifyErrors,
    MSALOauth2Provider * oauth2Provider
)
```


### function msalErrorFromMsidError:classifyErrors:msalOauth2Provider:correlationId:authScheme:popManager:

```objective-c
static virtual NSError * msalErrorFromMsidError:classifyErrors:msalOauth2Provider:correlationId:authScheme:popManager:(
    NSError * msidError,
    BOOL shouldClassifyErrors,
    MSALOauth2Provider * oauth2Provider,
    NSUUID * correlationId,
    id< MSALAuthenticationSchemeProtocol > authScheme,
    MSIDDevicePopManager * popManager
)
```


### function errorWithDomain:code:errorDescription:oauthError:subError:underlyingError:correlationId:userInfo:classifyErrors:msalOauth2Provider:authScheme:popManager:

```objective-c
static virtual NSError * errorWithDomain:code:errorDescription:oauthError:subError:underlyingError:correlationId:userInfo:classifyErrors:msalOauth2Provider:authScheme:popManager:(
    NSString * domain,
    NSInteger code,
    NSString * errorDescription,
    NSString * oauthError,
    NSString * subError,
    NSError * underlyingError,
    NSUUID * correlationId,
    NSDictionary * userInfo,
    BOOL shouldClassifyErrors,
    MSALOauth2Provider * oauth2Provider,
    id< MSALAuthenticationSchemeProtocol > authScheme,
    MSIDDevicePopManager * popManager
)
```


-------------------------------

Updated on 2023-10-18 at 16:35:54 -0700