---
title: Identity Zero Trust integration overview
description: Independent software vendors (ISVs) can integrate their solutions with Azure Active Directory to help customers adopt a Zero Trust model and keep their organizations secure.
ms.date: 09/17/2021
ms.service: security
author: knicholasa
ms.author: nichola
ms.topic: conceptual
---

# Identity integrations

:::image type="icon" source="../media/icon-identity-medium.png":::

Identities represent people, services, devices and the permissions that they obtain to access protected data. Identity solutions are the key control plane for managing access in the modern workplace and are essential to implementing Zero Trust.

Identity Zero Trust solutions enable customers to verify explicitly with strong authentication and access policies, use least privileged access with granular permission and access, and assume breach with controls and policies that manage access to secure resources and minimize the blast radius of attacks.

This guidance is for software providers and technology partners who want to enhance their identity security solutions by integrating with Microsoft products.

## Identity Zero Trust integration guide

This integration guide covers Azure Active Directory as well as Azure Active Directory B2C.

Azure Active Directory is Microsoft's cloud-based identity and access management service. It provides single sign-on authentication, conditional access, passwordless and multi-factor authentication, automated user provisioning and many more features that enable enterprises to protect and automate identity processes at scale.

Azure Active Directory B2C is a business-to-customer identity access management (CIAM) solution which customers use to implement secure white-label authentication solutions that scale easily and blend in with branded web and mobile application experiences. The integration guidance is available be in the [Azure AD B2C](#azure-ad-b2c) section below.

## Azure Active Directory

Azure Active Directory (Azure AD) is Microsoft’s cloud enterprise identity service. It provides conditional access to resources by using myriad input signals and enforcing policies set by organizations. As an independent software vendor (ISV) or technology partner, you can integrate with many of Azure AD’s features by using [Microsoft Graph](/graph/overview) and the [Microsoft identity platform](/azure/active-directory/develop/app-resilience-continuous-access-evaluation). You can also gain access to the risk signals Azure AD calculates and feed your own information back into the system to create a safer environment for all.

:::image type="content" source="../media/integrate/identity/diagram-conditional-access-policies.png" alt-text="Diagram of Zero Trust access control. Azure Active Directory sits in the middle, and is connected to input signals from partners and other Microsoft products. It makes policy decisions, and then allows access to 3rd party SaaS apps and devices directly or through other Microsoft products." border="true" lightbox="../media/integrate/identity/diagram-conditional-access-policies-expanded.png":::

In addition, there are other ways to provide value on top of Azure AD - such as publishing your app to the Azure AD gallery or becoming an approved passwordless hardware vendor.

### Getting started integrating Azure AD with your app

The modern threat landscape is complex, and there are many integration options to evaluate. This guidance is meant as a starting point for any ISV that wants to integrate with Azure AD to improve identity security.

You can use this section as inspiration and a starting point. The top level guidance is to:

- List your app in the Azure AD App Gallery
- Integrate user provisioning
- Integrate with key security APIs

After this, you will find [advanced integration scenario](#advanced-integration-scenarios) guidance for deeper integrations on specific solutions.

#### List your app in the Azure AD App Gallery

Publishing your app in [the app gallery](https://www.microsoft.com/security/business/identity-access-management/integrated-apps-azure-ad) will increase customer trust. Customers will know that your application has been validated as compatible with Azure Active Directory, and you can become a [verified publisher](/azure/active-directory/develop/publisher-verification-overview) so that customers are certain you are the publisher of the app they are adding to their tenant.

In addition, publishing in the app gallery will make it easy for IT admins to integrate the solution into their tenant with automated app registration. Manual registrations are a common cause of support issues with applications. Adding your app to the gallery will avoid these issues with your app.

#### Integrate user provisioning

Managing identities and access for organizations with thousands of users is challenging. If your solution will be used by large organizations, synchronizing information about users and access between your application and Azure AD is essential. This allows you to effectively scale your identity management systems on both cloud-only and hybrid environments as customers increase their dependence on the cloud.

SCIM (System for Cross-Domain Identity Management) is an open standard for exchanging user identity information. You can use the SCIM user management API to automatically provision users and groups between your application and Azure AD.

Our tutorial on the subject, [develop a SCIM endpoint for user provisioning to apps from Azure AD](/azure/active-directory/app-provisioning/use-scim-to-provision-users-and-groups), describes how to build a SCIM endpoint and integrate with the Azure AD provisioning service. The SCIM specification provides a common user schema for provisioning. When used in conjunction with federation standards like SAML or OpenID Connect, SCIM gives administrators an end-to-end, standards-based solution for access management.

#### Integrate with key security APIs

In our experience, many security ISVs have found these APIs to be particularly useful in improving their security solution and helping customers achieve Zero Trust.

##### User and group APIs

When you integrate User Provisioning, you can keep your application up to date with changes in the tenant where your application resides. But if your application needs to make updates to the users and groups in the tenant, you can use the User and Group APIs through Microsoft Graph to write back to the Azure Active Directory tenant. You can read more about using the API in the [Microsoft Graph REST API v1.0 reference](/graph/api/overview) in the reference documentation for [/graph/api/user-getmembergroups](/graph/api/user-getmembergroups)

##### Conditional Access API

[Conditional access](/azure/active-directory/conditional-access/overview) is a key part of Zero Trust because it helps to ensure the right user has the right access to the right resources. Enabling Conditional Access allows Azure Active Directory to make access decision based on computed risk and preconfigured policies.

ISVs can take advantage of conditional access by surfacing the option to apply conditional access policies when relevant to the customers. For example, if a user is especially risky, you can suggest the customer enable Conditional Access for that user through your UI, and programmatically enable it in Azure Active Directory.

:::image type="content" source="../media/integrate/identity/diagram-access-control.png" alt-text="Diagram showing a user using an application, which then calls Azure Active Directory to set conditions for a conditional access policy based on the user activity." border="true" lightbox="../media/integrate/identity/diagram-access-control-expanded.png":::

For more, check out the [configure conditional access policies using the Microsoft Graph API](https://github.com/Azure-Samples/azure-ad-conditional-access-apis/tree/main/01-configure/graphapi) sample on GitHub.

##### Confirm compromise and risky user APIs

Sometimes ISVs may become aware of compromise that is outside of the scope of Azure Active Directory. For any security event, especially those including account compromise, Microsoft and the ISV can collaborate by sharing information from both parties. The [confirm compromise API](/graph/api/riskyusers-confirmcompromised) allows you to set a targeted user’s risk level to high. This lets Azure Active Directory respond appropriately, for example by requiring the user to reauthenticate or by restricting their access to sensitive data.

:::image type="content" source="../media/integrate/identity/diagram-confirm-compromise.png" alt-text="Diagram showing a user using an application, which then calls Azure Active Directory to set user risk level to high." border="true" lightbox="../media/integrate/identity/diagram-confirm-compromise-expanded.png":::

Going in the other direction, Azure AD continually evaluates user risk based on various signals and machine learning. The Risky User API provides programmatic access to all at-risk users in the app’s Azure Active Directory tenant. ISVs can make use of this API to ensure they are handling users appropriately to their current level of risk. [riskyUser resource type](/graph/api/resources/riskyuser).

:::image type="content" source="../media/integrate/identity/diagram-risky-user.png" alt-text="Diagram showing a user using an application, which then calls Azure Active Directory to retrieve the user's risk level." border="true" lightbox="../media/integrate/identity/diagram-risky-user-expanded.png":::

### Advanced integration scenarios

In addition to the getting started guidance above, ISVs who are creating identity solutions can integrate with Azure AD to expand what they offer to customers. The following guidance is for ISVs who offer specific kinds of solutions and wish to integrate with Azure AD.

[Secure hybrid access integrations](/azure/active-directory/manage-apps/secure-hybrid-access-integrations)
Many business applications were created to work inside of a protected corporate network, and some of these applications make use of legacy authentication methods. As companies look to build a Zero Trust strategy and support hybrid and cloud-first work environments, they need solutions that connect apps to Azure Active Directory and provide modern authentication solutions for legacy applications. Use this guide to create solutions that provide modern cloud authentication for legacy on-premises applications.

[Become a Microsoft-compatible FIDO2 security key vendor](/azure/active-directory/authentication/concept-fido2-hardware-vendor)
FIDO2 security keys can replace weak credentials with strong hardware-backed public/private-key credentials which cannot be reused, replayed, or shared across services. You can become a Microsoft-compatible FIDO2 security key vendor by following the process in this document.

## Azure AD B2C

Azure AD B2C is a customer identity and access management (CIAM) solution capable of supporting millions of users and billions of authentications per day. It is a white-label authentication solution that enables user experiences which blend seamlessly with branded web and mobile applications.

Independent software vendors (ISVs) can integrate their own products and services with the identity and access management capabilities of Azure AD B2C. As with Azure AD, partners can integrate with Azure AD B2C by using [Microsoft Graph](/azure/active-directory-b2c/microsoft-graph-operations) and key security APIs such as the Conditional Access, confirm compromise, and risky user APIs. In addition, this section includes several other integration opportunities ISV partners can support.

We highly recommend customers using Azure AD B2C (and solutions that are integrated with it) activate [Identity Protection and Conditional Access in Azure AD B2C](/azure/active-directory-b2c/conditional-access-identity-protection-overview).

### Integrate with RESTful endpoints

ISVs can integrate their solutions via RESTful endpoints to enable multifactor authentication (MFA) , do role-based access control (RBAC), enable identity verification and proofing, improve security with bot detection and fraud protection, and meet Payment Services Directive 2 (PSD2) Secure Customer Authentication (SCA) requirements.

You can find [guidance on how to use our RESTful endpoints](/azure/active-directory-b2c/api-connectors-overview?pivots=b2c-user-flow) to integrate your applications with B2C capabilities in the B2C documentation. In addition, we have published detailed sample walkthroughs of partners who have integrated using the RESTful APIs:

- [Identity verification and proofing](/azure/active-directory-b2c/partner-gallery#identity-verification-and-proofing), which enables customers to verify the identity of their end users
- [Role-based access control](/azure/active-directory-b2c/partner-gallery#role-based-access-control), which enables granular access control to end users
- [Secure hybrid access to on-premises application](/azure/active-directory-b2c/partner-gallery#role-based-access-control), which enables end users to access on-premises and legacy applications with modern authentication protocols
- [Fraud protection](/azure/active-directory-b2c/partner-gallery#fraud-protection), which enables customers to protect their applications and end users from fraudulent login attempts and bot attacks

### Web application firewall

Web Application Firewall (WAF) provides centralized protection for web applications from common exploits and vulnerabilities. Azure AD B2C enables ISVs to integrate their WAF service such that all traffic to Azure AD B2C custom domains (for example, login.contoso.com) always pass through the WAF service, providing an additional layer of security.

Implementing a WAF solution requires that you configure Azure AD B2C custom domains. You can read how to do this in our [tutorial on enabling custom domains](/azure/active-directory-b2c/custom-domain?pivots=b2c-user-flow). You can also [see existing partners who have created WAF solutions that integrate with Azure AD B2C](/azure/active-directory-b2c/partner-gallery#web-application-firewall).

## Next steps

- [What is Azure Active Directory?](/azure/active-directory/fundamentals/active-directory-whatis)
- [ISV Partner gallery for Azure AD B2C](/azure/active-directory-b2c/partner-gallery)
- [Identity Protection and Conditional Access for Azure AD B2C](/azure/active-directory-b2c/conditional-access-identity-protection-overview)
