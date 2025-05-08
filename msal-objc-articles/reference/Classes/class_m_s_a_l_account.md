---
title: MSALAccount

---

# MSALAccount





## Public Functions

|                | Name           |
| -------------- | -------------- |
| virtual instancetype | **[initWithUsername:homeAccountId:environment:tenantProfiles:](Classes/class_m_s_a_l_account.md#function-initwithusername:homeaccountid:environment:tenantprofiles:)**(NSString * username, [MSALAccountId](Classes/class_m_s_a_l_account_id.md) * homeAccountId, NSString * environment, NSArray< [MSALTenantProfile](Classes/class_m_s_a_l_tenant_profile.md) * > * tenantProfiles) |
| virtual instancetype | **[initWithMSIDAccount:createTenantProfile:](Classes/class_m_s_a_l_account.md#function-initwithmsidaccount:createtenantprofile:)**(MSIDAccount * account, BOOL createTenantProfile) |
| virtual instancetype | **[initWithMSALExternalAccount:oauth2Provider:](Classes/class_m_s_a_l_account.md#function-initwithmsalexternalaccount:oauth2provider:)**(id< [MSALAccount](Classes/class_m_s_a_l_account.md) > externalAccount, MSALOauth2Provider * oauthProvider) |
| virtual void | **[addTenantProfiles:](Classes/class_m_s_a_l_account.md#function-addtenantprofiles:)**(NSArray< [MSALTenantProfile](Classes/class_m_s_a_l_tenant_profile.md) * > * tenantProfiles) |

## Public Properties

|                | Name           |
| -------------- | -------------- |
| [MSALAccountId](Classes/class_m_s_a_l_account_id.md) * | **[homeAccountId](Classes/class_m_s_a_l_account.md#property-homeaccountid)**  |
| NSString * | **[username](Classes/class_m_s_a_l_account.md#property-username)**  |
| NSString * | **[environment](Classes/class_m_s_a_l_account.md#property-environment)**  |
| NSMutableDictionary< NSString *, [MSALTenantProfile](Classes/class_m_s_a_l_tenant_profile.md) * > * | **[mTenantProfiles](Classes/class_m_s_a_l_account.md#property-mtenantprofiles)**  |
| NSDictionary< NSString *, NSString * > * | **[accountClaims](Classes/class_m_s_a_l_account.md#property-accountclaims)**  |
| NSString * | **[identifier](Classes/class_m_s_a_l_account.md#property-identifier)**  |
| MSIDAccountIdentifier * | **[lookupAccountIdentifier](Classes/class_m_s_a_l_account.md#property-lookupaccountidentifier)**  |
| BOOL | **[isSSOAccount](Classes/class_m_s_a_l_account.md#property-isssoaccount)**  |

## Public Functions Documentation

### function initWithUsername:homeAccountId:environment:tenantProfiles:

```objective-c
virtual instancetype initWithUsername:homeAccountId:environment:tenantProfiles:(
    NSString * username,
    MSALAccountId * homeAccountId,
    NSString * environment,
    NSArray< MSALTenantProfile * > * tenantProfiles
)
```


### function initWithMSIDAccount:createTenantProfile:

```objective-c
virtual instancetype initWithMSIDAccount:createTenantProfile:(
    MSIDAccount * account,
    BOOL createTenantProfile
)
```


**Parameters**: 

  * **account** MSID account 
  * **createTenantProfile** Whether to create tenant profile based on the info of MSID account 


Initialize an [MSALAccount](Classes/class_m_s_a_l_account.md) with MSIDAccount 


### function initWithMSALExternalAccount:oauth2Provider:

```objective-c
virtual instancetype initWithMSALExternalAccount:oauth2Provider:(
    id< MSALAccount > externalAccount,
    MSALOauth2Provider * oauthProvider
)
```


### function addTenantProfiles:

```objective-c
virtual void addTenantProfiles:(
    NSArray< MSALTenantProfile * > * tenantProfiles
)
```


## Public Property Documentation

### property homeAccountId

```objective-c
MSALAccountId * homeAccountId;
```


### property username

```objective-c
NSString * username;
```


### property environment

```objective-c
NSString * environment;
```


### property mTenantProfiles

```objective-c
NSMutableDictionary< NSString *, MSALTenantProfile * > * mTenantProfiles;
```


### property accountClaims

```objective-c
NSDictionary< NSString *, NSString * > * accountClaims;
```


### property identifier

```objective-c
NSString * identifier;
```


### property lookupAccountIdentifier

```objective-c
MSIDAccountIdentifier * lookupAccountIdentifier;
```


### property isSSOAccount

```objective-c
BOOL isSSOAccount;
```


-------------------------------

Updated on 2023-10-18 at 16:35:54 -0700