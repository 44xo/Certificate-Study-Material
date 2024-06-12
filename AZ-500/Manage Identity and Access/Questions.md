### Manage Microsoft Entra identities
1. A company wants to collaborate with a vendor outside of their organization. Which capability of Microsoft Entra External ID should they use?

B2B collaboration

Correct. B2B collaboration allows external users to sign in to the organization using their own credentials and access the apps and resources shared with them.

2. An organization wants to enable automatic remediation for identity-based risks detected by Microsoft Entra ID Protection. What access controls can be required based on the detected risk level?

Providing a strong authentication method, performing multifactor authentication, or performing a secure password reset

Correct. Based on the detected risk level, risk-based Conditional Access policies can be enabled to require access controls such as providing a strong authentication method, perform multifactor authentication, or perform a secure password reset.

3. A User Administrator wants to add a new user to their Microsoft Entra ID organization. What steps should they follow?

Sign in to the Azure portal in the User Administrator role, navigate to Microsoft Entra ID Users, and select either Create new user or Invite external user from the menu.

Correct. The User Administrator must sign in to the Azure portal in the User Administrator role, navigate to Microsoft Entra ID Users, and select either Create new user or Invite external user from the menu to add a new user to their Microsoft Entra ID organization.

4. A company wants to manage identity and access for external users at scale by automating access request workflows, access assignments, reviews, and expiration. Which feature should they use?

Microsoft Entra entitlement management

Correct. Microsoft Entra entitlement management is an identity governance feature that lets you manage identity and access for external users at scale by automating access request workflows, access assignments, reviews, and expiration.

5. A company wants to create a sign-up experience for external users who want to access their apps. What options can they provide as part of the sign-up flow?

Providing options for different social or enterprise identity providers.

Correct. As part of the sign-up flow, a company can provide options for different social or enterprise identity providers.

### Manage Microsoft Entra authentication


1. What is the main responsibility of the DC Agent service in Microsoft Entra Password Protection?

To process password validation requests from the password filter DLL of the DC Agent using the current locally available password policy and return the result of pass or fail

Correct. The DC Agent service processes password validation requests from the password filter DLL of the DC Agent using the current locally available password policy and returns the result of pass or fail.

2. A verifier uses their own Microsoft Entra tenant to perform the verification of the credential that was issued by another organization. What is the purpose of using Microsoft Entra Verified ID service for verification?

To unlock privileges to subjects that possess verified credential expert cards

Correct. The purpose of using Microsoft Entra Verified ID service for verification is to unlock privileges to subjects that possess verified credential expert cards.

3. A company is planning to deploy SSO with their applications in Microsoft Entra ID. They need to consider administrative roles, certificate renewal, communication, and licensing. Which of the following is true about the SAML application certificate?

The certificate needs to be renewed prior to its expiration, and the expiration date can be customized in the Microsoft Entra admin center.

Correct. It's important to have processes in place to renew certificates prior to their expiration, and to identify the right roles and email distribution lists involved with managing the lifecycle of the signing certificate. The certificate duration can be changed in the Microsoft Entra admin center.

4. What does the Linked option do in My Apps?

Lets you configure the target location when a user selects the application in your organization's end user portals

Correct. The Linked option lets you configure the target location when a user selects the application in your organization's end user portals.

5. What is the purpose of Microsoft Entra multifactor authentication?

To add additional security over only using a password when a user signs in

Correct. Microsoft Entra multifactor authentication adds additional security over only using a password when a user signs in by prompting the user for additional forms of authentication, such as responding to a push notification or entering a code from a software or hardware token.

### Manage Microsoft Entra authorization
1. A Privileged Role Administrator needs to assign a role to a user. What are the two main steps in the role assignment process?

Select the role to assign and adjust the role settings and duration.

Correct. The two main steps in the role assignment process are selecting the role to assign and adjusting the role settings and duration.

2. An organization wants to regularly review access to privileged Azure resources and Microsoft Entra ID roles. What tool can they use to create access reviews for privileged access?

Microsoft Entra Privileged Identity Management PIM

Correct. Microsoft Entra Privileged Identity Management PIM can be used to create access reviews for privileged access to Azure resources and Microsoft Entra ID roles.

3. A team lead needs to grant access to a specific resource group in Azure. What is the first step they should take?

Identify the needed scope by searching for the resource group in the Azure portal.

Correct. The first step is to identify the needed scope by searching for the resource group in the Azure portal.

4. A company wants to enable Permissions Management for their organization. What is the first step they need to take?

Ensure they have a Microsoft Entra ID tenant and are eligible for or have an active assignment to the Permissions Management Administrator role

Correct. The first step to enable Permissions Management is to ensure they have a Microsoft Entra ID tenant and are eligible for or have an active assignment to the Permissions Management Administrator role.

5. A security administrator needs to add a role assignment for a new user. Which step should they take to add a condition to the role assignment?

Select the Constrained recommended option under Delegation type on the Conditions tab.

Correct. This option provides more fine-grained access control by constraining the roles and principals the user can assign roles to.

### Manage Microsoft Entra application access
1. A developer wants to provide permissions-based access to their web API's resources to authorized users and client apps. What is the first step they need to take?

Register the web API with the Microsoft identity platform

Correct. To provide scoped access to the resources in your web API, you first need to register the API with the Microsoft identity platform.

2. An organization wants to easily achieve the right access policies for their configured applications. Which Microsoft service can help them achieve this?

Microsoft Entra ID

Correct. Microsoft Entra ID supports extensive access management for configured applications, enabling organizations to easily achieve the right access policies ranging from automatic, attribute based assignment (ABAC) or RBAC scenarios through delegation and including administrator management. With Microsoft Entra ID, usage and assignment reporting is fully integrated, enabling administrators to easily report on assignment state, assignment errors, and even usage.

3. A company wants to ensure that their web application is secure and protected from unauthorized access. They want to implement a security mechanism that will allow users to authenticate themselves before accessing the application. What is the first step they need to take?

Implement an authentication mechanism such as OAuth or OpenID Connect

Correct. Implementing an authentication mechanism such as OAuth or OpenID Connect is the first step in ensuring that users are authenticated before accessing the web application, thereby protecting it from unauthorized access.

4. A developer wants to provide permissions-based access to their web API. What steps should they take to achieve this?

The developer should register the web API with the Microsoft identity platform, assign an owner, create an app role, and add a scope.

Correct. By registering the web API and exposing it through scopes, assigning an owner and app role, the developer can provide permissions-based access to authorized users and client apps that access the API.

5. A developer needs to securely store secrets in Azure Key Vault and allow services to access them. What is the benefit of using managed identities?

Managed identities eliminate the need for developers to manage secrets, credentials, certificates, and keys used to secure communication between services.

Correct. Managed identities provide an automatically managed identity in Microsoft Entra ID for applications to use when connecting to resources that support Microsoft Entra authentication. Applications can use managed identities to obtain Microsoft Entra tokens without having to manage any credentials.