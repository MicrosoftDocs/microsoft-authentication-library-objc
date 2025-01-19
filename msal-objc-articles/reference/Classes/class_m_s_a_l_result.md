---
title: MSALResult

---

# MSALResult





## Public Functions

|                | Name           |
| -------------- | -------------- |
| virtual [MSALResult](Classes/class_m_s_a_l_result.md) * | **[resultWithMSIDTokenResult:authority:authScheme:popManager:error:](Classes/class_m_s_a_l_result.md#function-resultwithmsidtokenresult:authority:authscheme:popmanager:error:)**(MSIDTokenResult * tokenResult, MSALAuthority * authority, id< MSALAuthenticationSchemeProtocol, [MSALAuthenticationSchemeProtocolInternal] > authScheme, MSIDDevicePopManager * popManager, NSError ** error) |

## Public Functions Documentation

### function resultWithMSIDTokenResult:authority:authScheme:popManager:error:

```objective-c
static virtual MSALResult * resultWithMSIDTokenResult:authority:authScheme:popManager:error:(
    MSIDTokenResult * tokenResult,
    MSALAuthority * authority,
    id< MSALAuthenticationSchemeProtocol, MSALAuthenticationSchemeProtocolInternal > authScheme,
    MSIDDevicePopManager * popManager,
    NSError ** error
)
```


-------------------------------

Updated on 2023-10-18 at 16:35:54 -0700