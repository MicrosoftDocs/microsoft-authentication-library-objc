---
title: MSALTokenParameters

---

# MSALTokenParameters





## Public Functions

|                | Name           |
| -------------- | -------------- |
| virtual instancetype | **[initWithScopes:](Classes/class_m_s_a_l_token_parameters.md#function-initwithscopes:)**(NSArray< NSString * > * NS_DESIGNATED_INITIALIZER) |

## Public Properties

|                | Name           |
| -------------- | -------------- |
| MSALTelemetryApiId | **[telemetryApiId](Classes/class_m_s_a_l_token_parameters.md#property-telemetryapiid)**  |

## Public Functions Documentation

### function initWithScopes:

```objective-c
virtual instancetype initWithScopes:(
    NSArray< NSString * > * NS_DESIGNATED_INITIALIZER
)
```


**Parameters**: 

  * **scopes** Permissions you want included in the access token received in the result in the completionBlock. Not all scopes are guaranteed to be included in the access token returned. 


Initialize a [MSALTokenParameters](Classes/class_m_s_a_l_token_parameters.md) with scopes.


## Public Property Documentation

### property telemetryApiId

```objective-c
MSALTelemetryApiId telemetryApiId;
```


-------------------------------

Updated on 2023-10-18 at 16:35:54 -0700