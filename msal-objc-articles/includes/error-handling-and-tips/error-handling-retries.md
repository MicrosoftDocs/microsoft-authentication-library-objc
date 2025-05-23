---
author: henrymbuguakiarie
ms.author: henrymbugua
ms.date: 05/23/2025
ms.service: msal
ms.subservice: msal-ios-mac
ms.topic: include
#Customer intent: 
# Purpose:
# Ingested by Microsoft identity platform articles in /articles/active-directory/develop/* that document the error handling retries for the different platforms.
---
## Retrying after errors and exceptions

You're expected to implement your own retry policies when calling MSAL. MSAL makes HTTP calls to the Microsoft Entra service, and occasionally failures can occur. For example the network can go down or the server is overloaded.  

### HTTP 429

When the Service Token Server (STS) is overloaded with too many requests, it returns HTTP error 429 with a hint about how long until you can try again in the `Retry-After` response field.
