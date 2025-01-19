---
title: MSALTenantProfile

---

# MSALTenantProfile





## Public Functions

|                | Name           |
| -------------- | -------------- |
| virtual instancetype | **[initWithIdentifier:tenantId:environment:isHomeTenantProfile:claims:](Classes/class_m_s_a_l_tenant_profile.md#function-initwithidentifier:tenantid:environment:ishometenantprofile:claims:)**(nonnull NSString * identifier, nonnull NSString * tenantId, nonnull NSString * environment, BOOL isHomeTenantProfile, nullable NSDictionary * claims) |

## Public Properties

|                | Name           |
| -------------- | -------------- |
| NSString * | **[identifier](Classes/class_m_s_a_l_tenant_profile.md#property-identifier)**  |
| NSString * | **[environment](Classes/class_m_s_a_l_tenant_profile.md#property-environment)**  |
| NSString * | **[tenantId](Classes/class_m_s_a_l_tenant_profile.md#property-tenantid)**  |
| BOOL | **[isHomeTenantProfile](Classes/class_m_s_a_l_tenant_profile.md#property-ishometenantprofile)**  |
| NSDictionary< NSString *, NSString * > * | **[claims](Classes/class_m_s_a_l_tenant_profile.md#property-claims)**  |

## Public Functions Documentation

### function initWithIdentifier:tenantId:environment:isHomeTenantProfile:claims:

```objective-c
virtual instancetype initWithIdentifier:tenantId:environment:isHomeTenantProfile:claims:(
    nonnull NSString * identifier,
    nonnull NSString * tenantId,
    nonnull NSString * environment,
    BOOL isHomeTenantProfile,
    nullable NSDictionary * claims
)
```


## Public Property Documentation

### property identifier

```objective-c
NSString * identifier;
```


### property environment

```objective-c
NSString * environment;
```


### property tenantId

```objective-c
NSString * tenantId;
```


### property isHomeTenantProfile

```objective-c
BOOL isHomeTenantProfile;
```


### property claims

```objective-c
NSDictionary< NSString *, NSString * > * claims;
```


-------------------------------

Updated on 2023-10-18 at 16:35:54 -0700