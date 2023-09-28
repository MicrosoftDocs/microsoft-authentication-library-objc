---
title: "Cloud instance discovery: Instance aware flow"
description: "Some apps might need to work with multiple cloud instances at the same time depending on the account. In that case, instance aware flow might be useful."
services: active-directory
author: Dickson-Mwendia
manager: CelesteDG

ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/22/2023
ms.author: dmwendia
ms.reviewer: oldalton, brianmel
ms.custom: aaddev
---

# Cloud instance discovery: Instance aware flow

Some apps might need to work with multiple cloud instances at the same time depending on the account.
In that case, instance aware flow might be useful.

Instance aware flow allows to perform sovereign cloud discovery in MSAL. This feature is supported end to end only for Microsoft internal services. 

If developer wants to support discovery of accounts from sovereign cloud, e.g. German cloud Blackforest, Chinese cloud Mooncake, etc., two things need to be done:

1. The following parameter must be passed as extra query parameter for the interactive acquire token call:

   `instance_aware : true`

Note: Undefined behavior might happen if user tries to log in an account from sovereign cloud using a worldwide authority.

```obj-c
NSDictionary *extraQueryParameters = @{@"instance_aware":@"true"};

[application acquireTokenForScopes:kScopes
                         loginHint:nil
                        uiBehavior:MSALUIBehaviorDefault
             extraQueryParameters:extraQueryParameters
                  completionBlock:^(MSALResult *result, NSError *error) {
            // Handle result
}];
```

Note: you might need to pass a different set of scopes for each sovereign cloud. The specific set of scopes depends on the resource that you're using. For example, you could use "https://graph.microsoft.com/user.read" in worldwide cloud, and "https://graph.microsoft.de/user.read" in German cloud.

2. There will be an `authority` property in the `MSALResult` returned in the above call. It may be different from the authority provided by developer. 

Developer should use such an `authority` for subsequent silent acquire token call. Otherwise, cached result might be not found in subsequent calls. 
