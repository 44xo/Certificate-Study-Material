# Manage Microsoft Entra identities

## What is Entra

**Overview**
- Cloud-based identity and access management.
- Access to external resources (Microsoft 365, Azure portal, SaaS apps).
- Access to internal resources (corporate intranet apps, custom cloud apps).

**Users and Roles**
- **IT Admins**:
  - Control app access, multifactor authentication.
  - Automate user provisioning, protect user identities.
  - Free 30-day trial (P1 or P2).
- **App Developers**:
  - Standards-based authentication, SSO.
  - Use Microsoft Entra APIs.
  - Free 30-day trial (P1 or P2).
- **Microsoft 365, Office 365, Azure, Dynamics CRM Users**:
  - Automatic Microsoft Entra tenant.

**Licenses**
- **Free**:
  - Basic management, directory sync, SSO.
- **P1**:
  - Hybrid access, advanced admin features.
- **P2**:
  - P1 features plus ID Protection, Privileged Identity Management.
- **Pay as you go**:
  - Additional features like B2C identity solutions.

**Key Features**
- **Application Management**: Manage cloud/on-prem apps, SSO.
- **Authentication**: Password reset, multifactor authentication.
- **B2B**: Manage guest users/external partners.
- **B2C**: Custom user sign-up/sign-in.
- **Conditional Access**: Manage cloud app access.
- **Device Management**: Manage device access to data.
- **Domain Services**: Join Azure VMs to a domain.
- **Hybrid Identity**: Single identity for all resources.
- **Identity Governance**: Access controls, reviews.
- **Identity Protection**: Detect/respond to vulnerabilities.
- **Managed Identities**: Managed identity for Azure services.
- **PIM**: Control/monitor organizational access.
- **Monitoring and Health**: Security and usage insights.
- **Workload Identities**: Authenticate software workloads.

**Terminology**
- **Identity**: Entity that can be authenticated.
- **Account**: Identity with associated data.
- **Microsoft Entra Account**: Identity created through Microsoft Entra or other services.
- **Roles**:
  - **Account Admin**: Manages all subscriptions.
  - **Service Admin**: Manages all Azure resources.
  - **Owner**: Manages resources with fine-grained access.
  - **Global Admin**: Manages tenant, assigns roles.
- **Azure Subscription**: Payment for Azure services.
- **Tenant**: Dedicated instance for an organization.
  - **Single Tenant**: Dedicated environment.
  - **Multitenant**: Shared environment.
- **Custom Domain**: Personalized domain names.
- **Microsoft Account (MSA)**: Personal accounts for consumer services.

## Create a New User in Microsoft Entra ID

**Steps to Create a New User**

1. **Sign In**
   - **Role**: Ensure you are signed in as at least a User Administrator.
   - **Navigate**: Go to Identity > Users > All users.

2. **Initiate User Creation**
   - **New User**: Select New user > Create new user.
   - **Complete Tabs**: Follow the steps in the New user page.

**Basics Tab**

- **User Principal Name (UPN)**:
  - **Format**: Enter a unique username and select a domain.
  - **New Domain**: Select Domain not listed if a new domain is needed (See: Add your custom domain name).
- **Mail Nickname**:
  - **Default**: Derived from UPN.
  - **Custom**: Uncheck the Derive from user principal name to enter a different nickname.
- **Display Name**: Enter the full name of the user (e.g., Chris Green).
- **Password**:
  - **Default**: Auto-generated.
  - **Custom**: Uncheck Auto-generate password to enter a custom password.
- **Account Enabled**:
  - **Default**: Enabled.
  - **Disable**: Uncheck to prevent immediate sign-in (was called Block sign in in legacy).

**Properties Tab**

- **Categories**: Identity, Job information, Contact information, Parental controls, Settings.
- **Details**:
  - **Identity**: Enter first and last names, set User type (Member or Guest).
  - **Job Information**: Add job title, department, manager.
  - **Contact Information**: Add relevant contact details.
  - **Parental Controls**: Specify age group (Minors, Not adult, Adults).
  - **Settings**: Specify global location.
- **Save**: Select Review + create or Next: Assignments to proceed.

**Assignments Tab**

- **Group Assignment**:
  - **Add Group**: Select + Add group, choose up to 20 groups, click Select.
- **Role Assignment**:
  - **Add Role**: Select + Add role, choose up to 20 roles, click Select.
- **Administrative Unit**:
  - **Add Unit**: Select + Add administrative unit, choose one unit, click Select.
- **Save**: Select Review + create to finalize.

**Finalizing User Creation**

- **Review Details**: Ensure all entered details are correct.
- **Create User**: Click Review + create to complete the process.

**Summary**

1. **Sign In**: User Administrator role required.
2. **Navigate**: Identity > Users > All users > New user > Create new user.
3. **Complete Basics**:
   - UPN, Mail Nickname, Display Name, Password, Account Enabled.
4. **Complete Properties**:
   - Identity, Job Information, Contact Information, Parental Controls, Settings.
5. **Assign Groups/Roles**:
   - Up to 20 groups/roles; one administrative unit.
6. **Review & Create**: Finalize user creation by selecting Review + create.

This cheat sheet outlines the key steps and options to efficiently create a new user in Microsoft Entra ID.
## Secure Microsoft Entra users

**User Management Overview**
- Create various user types for flexible management.
- Follow GDPR guidelines for personal data management.

**Prerequisites**
- Use the least privileged role necessary.
- **Global Administrator**: Create users and assign roles (use minimally).

**Roles and Tasks**
- **User Administrator**: Create new users.
- **Guest Inviter**: Invite external guests.
- **Privileged Role Administrator**: Assign Microsoft Entra roles.

**User Types**
- **Internal Member**:
  - Full-time employees.
- **Internal Guest**:
  - Account in your tenant with guest-level privileges.
- **External Member**:
  - External account with member access in your tenant (common in multitenant setups).
- **External Guest**:
  - Authenticates externally with guest-level privileges.

**Authentication Methods**
- **Internal Guests/Members**:
  - Credentials managed within Microsoft Entra tenant.
  - Can reset own passwords.
- **External Members**:
  - Authenticate via home Microsoft Entra tenant.
  - Password resets managed by their home tenant admin.
- **External Guests**:
  - Set up passwords via email link when account is created.

**Notes**
- **Global Administrator**: Use least privilege role possible for security.
- Review user types and their specific access needs before creation or invitation.
## Secure Microsoft Entra groups

**Overview**
- Manage access to resources, applications, and tasks via groups.
- Supports Zero Trust security principle by limiting access to necessary users.

**Types of Resources Managed**
- Microsoft Entra organization permissions.
- External SaaS apps.
- Azure services.
- SharePoint sites.
- On-premises resources.

**Management Restrictions**
- On-premises AD groups: Managed only in on-premises AD.
- Distribution lists and mail-enabled security groups: Managed in Exchange or Microsoft 365 admin center.

**Group Types**
1. **Security**:
   - Manages user and computer access to shared resources.
   - Members: Users, devices, service principals, nested groups.
   - Owners: Users, service principals.
   - Note: Nested group members don't inherit parent group access.
2. **Microsoft 365**:
   - Provides shared mailbox, calendar, files, SharePoint access.
   - Members: Users only.
   - Owners: Users, service principals.

**Membership Types**
1. **Assigned**:
   - Add specific users manually.
2. **Dynamic User**:
   - Auto-add/remove users based on attribute rules.
3. **Dynamic Device**:
   - Auto-add/remove devices based on attribute rules.

**Access Rights Management**
- Apply the principle of least privilege.
- Each app/resource/service needs separate permission management.

**Assigning Access Rights**
1. **Direct Assignment**:
   - Resource owner assigns individual users.
2. **Group Assignment**:
   - Assigns a group to a resource, granting all members access.
   - Group/resource owners manage membership.
3. **Rule-based Assignment**:
   - Assigns users based on attribute rules.
4. **External Authority Assignment**:
   - External source (e.g., on-premises directory) manages access.

**User-Initiated Group Joining**
- Group owners can allow users to request group membership.
- Options for auto-approval or requiring owner approval.
- Owners notified of join requests and can approve or deny.

**Best Practices**
- Use least privilege role for security.
- Review group types and membership before creation.
- Manage permissions separately for each resource.

**Key Roles**
- **User Administrator**: Create new users.
- **Guest Inviter**: Invite external guests.
- **Privileged Role Administrator**: Assign roles.

Keep these points in mind for effective management and security of Microsoft Entra groups.
## Recommend when to use external identities

**Overview**
- **Microsoft Entra B2B Collaboration**: Invite guest users for secure collaboration using their own credentials.
- Control over corporate data while enabling external user access.

**Invitation Process**
- **Simple Invitation**: Partners use their own credentials.
- **Self-Service Sign-Up**: External users can sign up for apps/resources themselves.
- **User Object Representation**: Guests appear as user objects in the directory, usually marked with "#EXT#".

**Customization and APIs**
- **Business-to-Business APIs**: Customize invitation processes and develop self-service portals.
- **Pricing Information**: Refer to Microsoft Entra External ID pricing for guest user costs.

**Benefits of B2B Collaboration**
- **Partner's Identity Management**: No need for external administrative overhead.
- **Use of Own Credentials**: Work, school, or social identities can be used.
- **No Account/Password Management**: External accounts are not managed by your organization.

**Admin Settings for B2B Collaboration**
- **Cross-Tenant Access Settings**: 
  - Manage inbound/outbound collaboration.
  - Scope access for specific users, groups, applications.
  - Trust MFA and device claims from other Microsoft Entra organizations.
- **External Collaboration Settings**:
  - Define who can invite external users.
  - Allow/block specific domains.
  - Restrict guest user directory access.
- **Microsoft Cloud Settings**:
  - Establish collaboration between different Azure environments.

**Inviting Guest Users**
- **Azure Portal**: Add guest users easily.
- **Invitation Process**:
  - Create guest user.
  - Assign to apps/groups.
  - Send invitation with redemption link.

**Self-Service Sign-Up**
- **Sign-Up User Flow**: Create sign-up experience for external users.
- **Integration Options**: Use API connectors for custom workflows, identity verification, and user validation.

**Security Policies**
- **Authentication and Authorization**: Use policies to protect content.
- **Conditional Access**: Enforce MFA at tenant, application, or user level.

**Delegated Guest User Management**
- **App/Group Owners**: Allow them to add guest users directly.
- **Self-Service Management**: Administrators set up, non-admins use Access Panel.

**Custom Onboarding**
- **Entitlement Management**: Configure policies for external user access.
- **Invitation APIs**: Customize onboarding experiences.

**Integration with Identity Providers**
- **Supported Providers**: Facebook, Microsoft accounts, Google, enterprise providers.
- **Federation Setup**: External users sign in with existing accounts.

**SharePoint and OneDrive Integration**
- **File Sharing**: Share resources with external users.
- **Email One-Time Passcode**: Fallback authentication method for guest users.

Keep these key points in mind for efficiently managing and securing external identities in your Microsoft Entra environment.
## Secure external identities

**Overview**
- **Microsoft Entra External ID**: Solutions for secure external identity access to apps/resources.
- **Supported Identities**: Corporate, government-issued, and social identities (Google, Facebook).

**Key Scenarios**
1. **Consumer Apps (CIAM)**:
   - Use External ID for authentication and customer identity management.
   - Create customized sign-in experiences.
   - Manage users in a separate tenant from employees.
2. **B2B Collaboration**:
   - Enable employees to collaborate with external partners.
   - Secure access through invitation/self-service sign-up.
   - Manage access levels within the same tenant as employees.

**Consumer and Business Customer Security**
- **External Tenant for CIAM**:
  - **Custom Registration**: Self-service registration with various sign-in methods.
  - **Company Branding**: Custom look and feel for sign-up/sign-in.
  - **User Attributes**: Collect built-in or custom attributes.
  - **User Insights**: Analyze activity and engagement data.
- **Benefits**: Enhanced security, compliance, scalability, and user convenience.

**B2B Collaboration Security**
- **Guest Access**:
  - Use external identities for access without managing credentials.
  - **Methods to Add Guests**:
    - Invitations via Microsoft Entra admin center or PowerShell.
    - Self-service sign-up flows.
    - Entitlement management for automated access workflows.
  - **User Management**:
    - Create user objects for guests, manage like employees.
    - Assign permissions while guests use their own credentials.
  - **Cross-Tenant Access Settings**:
    - Manage collaboration with other Microsoft Entra organizations and Azure clouds.

**Tenant Configurations**
1. **Workforce Tenant**:
   - Standard tenant for employees and internal resources.
   - Supports B2B collaboration.
2. **External Tenant**:
   - Separate tenant for consumer/business customer apps.
   - Manages users independently from employee directory.

**Feature Comparison**

| Identity                | External ID in Workforce Tenants                   | External ID in External Tenants                        |
|-------------------------|----------------------------------------------------|--------------------------------------------------------|
| Primary Scenario        | Collaborate with business guests                   | Publish apps for consumers/business customers          |
| Intended For            | External partners like suppliers, vendors          | Consumers/business customers of your app               |
| User Management         | Manage guests like employees, use cross-tenant settings | Managed in external tenant, separate from employee directory |
| Single Sign-On (SSO)    | Supported for Microsoft and SaaS apps              | Supported for apps registered in external tenant       |
| Company Branding        | Customizable guest sign-in                         | Customizable branding for external apps                |
| Microsoft Cloud Settings| Supported                                          | Not applicable                                         |
| Entitlement Management  | Supported                                          | Not applicable                                         |

**Related Technologies**
- **B2B Direct Connect**:
  - Two-way trust relationships for Teams shared channels.
  - Users authenticate in their home organization.
  - Managed through cross-tenant access settings.
- **Azure Active Directory B2C**:
  - Legacy solution for customer identity management.
  - Separate from Microsoft Entra tenants.
- **Entitlement Management**:
  - Self-service sign-up for external collaborators.
  - Automated guest account provisioning and access management.
  -  "Is an identity governance feature that lets you manage identity and access for external users at scale by automating access request workflows, access assignments, reviews, and expiration."
- **Microsoft Graph API**:
  - APIs for cross-tenant settings and invitation management.
- **Conditional Access**:
  - Enforce policies for B2B collaboration/direct connect users.
  - Trust MFA and device claims from external users’ home organizations.
- **Multitenant Applications**:
  - Cross-tenant synchronization for seamless collaboration.
  - Configured under organization-specific access settings.

**Licensing and Billing**
- Based on monthly active users (MAU).
- Refer to External ID pricing and billing setup for B2B.

Keep these key points in mind for managing and securing external identities in your Microsoft Entra environment.
## Implement Microsoft Entra ID Protection

**Overview**
- **Microsoft Entra ID Protection**: Detects, investigates, and remediates identity-based risks.
- **Integration**: Works with Conditional Access for access decisions or feeds data to SIEM tools.

**Risk Detection**
- **Detection Sources**: Analyzes signals from Active Directory, Microsoft Accounts, Xbox.
- **Types of Risks Detected**:
  - Anonymous IP address usage
  - Password spray attacks
  - Leaked credentials
- **Real-time Sign-in Detection**: Assesses risk level for each sign-in, applying appropriate policies.

**Investigation**
- **Key Reports**:
  - **Risk Detections**: Tracks each detected risk.
  - **Risky Sign-ins**: Reports sign-ins with one or more risk detections.
  - **Risky Users**: Identifies users with risky sign-ins or other risk detections.

**Remediation**
1. **Automatic Remediation**:
   - **Risk-based Conditional Access Policies**: Enforce strong authentication, MFA, or password resets.
   - **Outcome**: Successful access control remediates the risk automatically.
2. **Manual Remediation**:
   - **Administrator Actions**: Dismiss, confirm safe, or confirm compromise.
   - **Access Points**: Portal, API, Microsoft 365 Defender.

**Data Utilization**
- **Exporting Data**:
  - Use Microsoft Graph APIs to collect data for SIEM tools.
  - **Integration**: Instructions available in articles for Microsoft Sentinel and risk data export.
- **Data Storage Options**:
  - Log Analytics workspace
  - Storage account
  - Event Hubs
  - Other solutions

**Required Roles**
- **Role Capabilities**:

| Role                   | Can Do                                           | Can't Do                                       |
|------------------------|--------------------------------------------------|------------------------------------------------|
| Security Administrator | Full access to Identity Protection               | Reset passwords                                |
| Security Operator      | View reports, dismiss risks, confirm safe/compromise | Configure/change policies, reset passwords     |
| Security Reader        | View reports                                     | Configure/change policies, reset passwords     |
| Global Reader          | Read-only access                                 | N/A                                            |
| User Administrator     | Reset passwords                                  | N/A                                            |

- **Note**: Security Operators cannot access the Risky sign-ins report.

**Conditional Access Policies**
- **Conditions**: Can include user or sign-in risk.
- **Details**: Refer to the article on Conditional Access: Conditions.

**License Requirements**
- **Entra ID P2 License Needed**:
  - **Features**: Risk policies, security reports, notifications, MFA registration policy.
  - **License Comparison**:

| Capability             | Details                                           | Free / M365 Apps | Entra ID P1 | Entra ID P2 |
|------------------------|--------------------------------------------------|------------------|-------------|-------------|
| Risk Policies          | Sign-in and user risk policies                   | No               | No          | Yes         |
| Security Reports       | Overview                                         | No               | No          | Yes         |
| Risky Users            | Limited info                                     | Yes (limited)    | Yes (limited)| Full access |
| Risky Sign-ins         | Limited info                                     | Yes (limited)    | Yes (limited)| Full access |
| Risk Detections        | Limited info                                     | No               | Yes (limited)| Full access |
| Notifications          | Users at risk alerts, Weekly digest              | No               | No          | Yes         |
| MFA Registration Policy|                                                   | No               | No          | Yes         |

**Workload Identity Risk**
- **Licensing**: Workload Identities Premium needed.
- **Features**: Risky workload identities, Workload identity detections.

Keep these key points in mind to effectively implement and manage Microsoft Entra Identity Protection in your organization.

# Manage Microsoft Entra authentication

## Microsoft Entra Connect
**Overview**

Microsoft Entra Connect is an on-premises application designed to integrate your on-premises Active Directory (AD) with Microsoft Entra ID to support a hybrid identity environment.

**Features of Microsoft Entra Connect**

1. **Password Hash Synchronization**
   - **Function**: Synchronizes a hash of users' on-premises AD passwords with Microsoft Entra ID.
   - **Benefit**: Simplifies sign-in by using the same password in both environments.

2. **Pass-through Authentication**
   - **Function**: Users use the same password on-premises and in the cloud without needing additional infrastructure.
   - **Benefit**: Provides seamless sign-in without federated environment requirements.

3. **Federation Integration**
   - **Function**: Configures a hybrid environment using on-premises AD FS infrastructure.
   - **Benefit**: Manages AD FS capabilities, including certificate renewal and server deployments.

4. **Synchronization**
   - **Function**: Creates users, groups, and synchronizes identity information between on-premises and cloud.
   - **Benefit**: Ensures consistency of identity information and password hashes.

5. **Health Monitoring**
   - **Function**: Provides robust monitoring via Microsoft Entra Connect Health.
   - **Benefit**: Centralized monitoring of key identity components, accessible in the Microsoft Entra admin center.

**Microsoft Entra Connect Health**

- **Purpose**: Monitors on-premises identity infrastructure to maintain reliable connections to Microsoft 365 and Microsoft Online Services.
- **Portal**: Displays alerts, performance data, usage analytics, and other critical information.
- **Key Features**:
  - **Alerts**: Get notifications on critical issues.
  - **Performance Monitoring**: Track performance and connectivity of identity components.
  - **Usage Analytics**: Analyze usage patterns and trends.

**Why Use Microsoft Entra Connect?**

- **Unified Identity**: Users can use a single identity for on-premises and cloud resources, enhancing productivity.
- **Ease of Deployment**: Single tool for synchronization and sign-in, replacing older tools like DirSync and Azure AD Sync.
- **Latest Capabilities**: Provides updated features and capabilities for hybrid identity scenarios.

**Why Use Microsoft Entra Connect Health?**

- **Productivity**: Common identity access improves user productivity.
- **Reliability**: Ensures consistent and reliable access to resources.
- **Monitoring**: Easy installation of Health Agent for AD FS monitoring and insights.

**Supported Platforms**

- **AD FS Support**: Supports Windows Server 2012 R2, 2016, 2019, and 2022.
- **Web Application Proxy**: Monitors servers providing extranet authentication.

**Key Benefits and Best Practices**

| **Key Benefits**                      | **Best Practices**                 |
|--------------------------------------|------------------------------------|
| Enhanced Security                    | Extranet lockout trends            |
| Failed sign-ins report               | Server configuration and availability |
| Privacy Compliant                     | Performance and connectivity       |
| Alerts on critical AD FS issues       | Regular maintenance                |
| Easy Deployment and Management        | Quick agent installation           |
| Rich Usage Metrics                    | Agent auto upgrade                 |
| Great User Experience                 | Data available in portal quickly   |
| Dashboard view in admin center        | Alerts via email                   |

## Implement multi-factor authentication (MFA)
### Implementing Multifactor Authentication (MFA) - Quick Guide

**Overview**
Multifactor authentication (MFA) enhances security by requiring additional forms of identification during sign-in, such as a code from a mobile phone or a fingerprint scan.

**Key Benefits**
- Increased security by adding an additional verification step.
- Flexibility to enforce MFA for specific sign-in events using Conditional Access policies.

**Prerequisites**
- Microsoft Entra ID P1 or trial licenses enabled.
- Administrator account with Conditional Access, Security, or Global Administrator privileges.
- A non-administrator account (e.g., testuser) and a group (e.g., MFA-Test-Group) for testing MFA.

**Setting Up MFA with Conditional Access**
1. **Create a Conditional Access Policy**
   - Sign in to Microsoft Entra admin center.
   - Navigate to **Protection > Conditional Access**, then select **+ New policy**.
   - Name the policy (e.g., MFA Pilot) and assign it to the test group (e.g., MFA-Test-Group).

2. **Configure Conditions for MFA**
   - Define cloud apps or actions that trigger MFA.
   - Example: Select Windows Azure Service Management API to apply the policy to sign-in events.

3. **Set Access Controls**
   - Under **Access controls > Grant**, select **Require multifactor authentication**.

4. **Activate the Policy**
   - Enable the policy and select **Create** to apply it.

**Testing MFA**
1. **Without MFA Requirement**
   - Open a browser in incognito mode and sign in to https://account.activedirectory.windowsazure.com using the test user.
   - Verify there is no MFA prompt.

2. **With MFA Requirement**
   - Sign in to the Microsoft Entra admin center using the test user.
   - Configure and use the selected MFA method (e.g., mobile app, phone call).

**Configuring MFA Settings**
1. **Account Lockout (MFA Server Only)**
   - Prevents repeated MFA attempts in an attack.
   - Configure in **Microsoft Entra ID > Security > Multifactor authentication > Account lockout**.

2. **Block/Unblock Users**
   - Temporarily block users if their device is lost/stolen.
   - Configure in **Microsoft Entra ID > Security > Multifactor authentication > Block and unblock users**.

3. **Fraud Alerts**
   - Allows users to report fraudulent MFA requests.
   - Configure in **Microsoft Entra ID > Security > Multifactor authentication > Report suspicious activity**.

4. **Notifications**
   - Set up email alerts for reported fraud events.
   - Example of a fraud alert notification email provided.

5. **OATH Tokens**
   - Support for OATH TOTP tokens for additional security.
   - Upload tokens in **Microsoft Entra ID > Security > Multifactor authentication > OATH tokens**.

**Managing Suspicious Activity**
- Investigate and remediate suspicious activity using Identity Protection.
- Enable reporting of suspicious activity in **Microsoft Entra ID > Security > Authentication Methods > Settings**.

This guide covers the essentials for implementing and managing multifactor authentication in Microsoft Entra, ensuring enhanced security and compliance for your organization's identity infrastructure.
## Configure Microsoft Entra Verified ID
#### Overview
In scenarios where the issuer and verifier are different organizations, the verifier uses their Microsoft Entra tenant to verify credentials issued by another organization. This guide helps you configure and test the Microsoft Entra Verified ID service using a sample application.
#### Purpose
 The purpose of using Microsoft Entra Verified ID service for verification is to unlock privileges to subjects that possess verified credential expert cards.
#### Prerequisites
- Microsoft Entra Verified ID tenant setup
- Git installed (if cloning the repository)
- Code editor (e.g., Visual Studio Code)
- .NET 7.0
- ngrok with a free account
- Latest Microsoft Authenticator app

#### Steps to Configure

1. **Gather Tenant Details**
   - In Microsoft Entra Verified ID, go to Organization settings.
   - Copy and record the Tenant identifier and Decentralized identifier.

2. **Download Sample Code**
   - Clone or download the sample application from the GitHub repository.

3. **Configure the Verifiable Credentials App**
   - Create a client secret in Microsoft Entra ID under App registrations for your verifiable-credentials-app.
   - Copy the Application (client) ID and the secret’s value.

4. **Update Sample Application**
   - In the `appsettings.json` file of the sample app, update with your Tenant ID, Client ID, Client Secret, DidAuthority, and CredentialType.

   ```json
   {
     "VerifiedID": {
       "Endpoint": "https://verifiedid.did.msidentity.com/v1.0/verifiableCredentials/",
       "VCServiceScope": "3db474b9-6a0c-4840-96ac-1fceb342124f/.default",
       "Instance": "https://login.microsoftonline.com/",
       "TenantId": "12345678-0000-0000-0000-000000000000",
       "ClientId": "33333333-0000-0000-0000-000000000000",
       "ClientSecret": "123456789012345678901234567890",
       "DidAuthority": "did:web:...your-decentralized-identifier...",
       "CredentialType": "VerifiedCredentialExpert",
       "CredentialManifest": "https://verifiedid.did.msidentity.com/v1.0/12345678-0000-0000-0000-000000000000/verifiableCredentials/contracts/VerifiedCredentialExpert"
     }
   }
   ```

5. **Run and Test the Sample App**
   - From Visual Studio Code, navigate to the project directory and run:

   ```bash
   cd active-directory-verifiable-credentials-dotnet\1-asp-net-core-api-idtokenhint
   dotnet build "AspNetCoreVerifiableCredentials.csproj" -c Debug -o .\bin\Debug\net6
   dotnet run
   ```

   - In another terminal, run ngrok to expose the local server:

   ```bash
   ngrok http 5000
   ```

   - Open the HTTPS URL generated by ngrok.

6. **Verify Credential**
   - In the web browser, select "Verify Credential."
   - Scan the QR code using Microsoft Authenticator.
   - Ignore the domain warning by selecting "Advanced" and then "Proceed anyways (unsafe)."
   - Approve the credential request in Authenticator.

7. **Check Results**
   - Verify the credential presentation in the sample app and view recent activities for the verifiable credential.

## Implement passwordless authentication

#### Overview
Passwords are a major vulnerability in security, susceptible to attacks such as social engineering, phishing, and spray attacks. A passwordless authentication strategy can mitigate these risks. Microsoft provides three main passwordless authentication methods integrated with Microsoft Entra ID:

1. **Microsoft Authenticator**: Transforms any iOS or Android phone into a strong, passwordless credential, allowing users to sign in to any platform or browser.
2. **FIDO2-Compliant Security Keys**: Ideal for shared machines like kiosks, situations with phone restrictions, and highly privileged identities.
3. **Windows Hello for Business**: Suitable for users on their dedicated Windows computers.

#### Using the Passwordless Methods Wizard
The Microsoft Entra admin center features a passwordless methods wizard to help select the appropriate method for different user groups.

#### Passwordless Authentication Scenarios
Selecting the right passwordless authentication strategy involves considering organizational needs, prerequisites, and method capabilities.

**Table: Passwordless Authentication Methods by Device Types**

| Device Types                                    | Passwordless Authentication Method                  |
|-------------------------------------------------|----------------------------------------------------|
| Dedicated non-Windows devices                   | Microsoft Authenticator, Security keys             |
| Dedicated Windows 10 computers (version 1703+)  | Windows Hello for Business, Security keys          |
| Dedicated Windows 10 computers (before 1703)    | Windows Hello for Business, Microsoft Authenticator app |
| Shared devices: tablets, mobile devices         | Microsoft Authenticator, One-time password sign-in  |
| Kiosks (Legacy)                                 | Microsoft Authenticator                            |
| Kiosks and shared computers (Windows 10)        | Security keys, Microsoft Authenticator app         |

#### Prerequisites
Ensure that prerequisites are met before starting passwordless deployment.

**Roles Required for Deployment**

| Microsoft Entra Role              | Description                                    |
|-----------------------------------|------------------------------------------------|
| User Administrator or Global Admin| To implement combined registration experience. |
| Authentication Administrator      | To implement and manage authentication methods.|
| User                              | To configure Authenticator app or enroll security key.|

Passwordless authentication should be enabled for all privileged accounts.

**Prerequisites by Method**

| Prerequisite                                       | Microsoft Authenticator | FIDO2 Security Keys |
|---------------------------------------------------|-------------------------|---------------------|
| Combined registration for MFA and SSPR enabled    | √                       | √                   |
| Users can perform MFA                             | √                       | √                   |
| Users have registered for MFA and SSPR            | √                       | √                   |
| Users have registered their mobile devices        | √                       |                     |
| Windows 10 version 1809+ and supported browser    |                         | √                   |
| Compatible FIDO2 security keys                    |                         | √                   |

#### Windows Hello for Business
Deployment paths for Windows Hello for Business depend on whether the setup is on-premises, hybrid, or cloud-only and the device join strategy. The wizard in the Microsoft Entra admin center will provide a step-by-step plan based on your inputs.

#### Project Planning
Engaging the right stakeholders and clearly defining their roles is crucial for the success of the deployment.

#### Pilot Program
Start by enabling passwordless authentication for one or more pilot groups to test the new methods.

#### Communication
Inform end users about the new authentication methods through guidance and communication templates provided by Microsoft. These materials include posters and email templates for notifying users about the upcoming changes.

#### User Registration
Users can register their passwordless methods as part of the combined security information workflow at [https://aka.ms/mysecurityinfo](https://aka.ms/mysecurityinfo).

**Temporary Access Passcode (TAP)**
Admins can provide a TAP for users to register their security information. This is a time-limited passcode suitable for first-time users or recovery scenarios.

#### Technical Considerations
**Active Directory Federation Services (AD FS) Integration**
- Hybrid tenants prevent users from being directed to AD FS for sign-in unless they select "Use your password instead."
- For non-Microsoft 365 applications using AD FS, set up access control policies within AD FS.

**MFA Server**
- End users enabled for MFA through an on-premises server can use a single passwordless phone sign-in credential.

**Device Registration**
- Devices must be registered in the Microsoft Entra tenant for passwordless authentication and cannot be shared devices.

### Deploy Microsoft Authenticator
- Download and enable the Microsoft Authenticator app for iOS or Android.
- Ensure device registration in the Microsoft Entra tenant.

**Test Cases for Microsoft Authenticator**
- User registration, enabling phone sign-in, accessing apps with phone sign-in, and rolling back phone sign-in registration.

### Deploy FIDO2-Compliant Security Keys
- Ensure compatible security keys.
- Plan the lifecycle including distribution, activation, and disabling keys.

**Technical Considerations for FIDO2 Security Keys**
- Requires Windows 10 version 1809 or higher.
- Enable the credential provider functionality using Microsoft Intune or Group Policy.
- Restrict keys to specific manufacturers if needed.

**Test Cases for Security Keys**
- Registration and sign-in scenarios for different Windows builds and web apps.

### Troubleshooting
Address common issues such as registration problems, security key management, and browser or OS support errors.

**Resources**
- Enable passwordless sign-in with Microsoft Authenticator.
- Enable passwordless security key sign-in.
- Troubleshoot common issues and access additional support resources.

## Implement password protection

#### Overview
Microsoft Entra Password Protection blocks weak passwords and their variants, applying both global and custom banned password lists for on-premises and cloud environments. This guide explains how to deploy and use Microsoft Entra Password Protection on-premises.

#### Design Principles
- Domain controllers (DCs) don’t need internet access.
- No new network ports on DCs.
- No schema changes in Microsoft Entra Domain Services.
- Works with any supported domain or forest functional level.
- Does not create or require new accounts in protected domains.
- User passwords never leave the DC.
- Independent of other Microsoft Entra features.
- Incremental deployment supported.

#### Incremental Deployment
- The DC agent validates passwords only on DCs where it's installed.
- Full deployment is necessary for consistent enforcement.
- Partial deployment can be used for testing but is not secure.

#### Components
- **Proxy Service**: Forwards password policy download requests from DCs to Microsoft Entra ID.
- **DC Agent**: Receives and processes password validation requests.

#### Architectural Diagram
A visual representation shows how Microsoft Entra Password Protection components interact.

#### How It Works
1. **Proxy Service Advertisement**: Proxy service advertises itself to DCs via a serviceConnectionPoint object.
2. **Policy Download**: DC Agent locates a proxy service, sends a policy download request, and stores the policy in the domain sysvol folder.
3. **Policy Updates**: DC Agent checks and updates the password policy hourly.
4. **Password Validation**: When passwords are changed, the DC uses the cached policy to accept or reject them.

#### Key Features
- Password policies combine global and custom banned-password lists.
- Communication between DC Agent and proxy service uses RPC over TCP.
- The proxy service is stateless and doesn't cache policies.
- The most recent locally available policy is always used.
- Microsoft Entra Password Protection supplements existing password policies.

#### Deployment Steps

1. **Download Required Installers**: Obtain the agent installers from the Microsoft Download Center.

2. **Register AD DS Forest and Proxy Services**:
   - Register the AD DS forest and proxy services with Microsoft Entra ID.
   - Ensure all proxy services in a forest are registered with the same Microsoft Entra tenant.

3. **Install Proxy Service**:
   - Install on any domain-joined machine within the Microsoft Entra ID forest.
   - Configure the service to forward policy requests.

4. **Install DC Agent**:
   - Install on all DCs where password validation is needed.
   - Configure the agent to use the locally available password policy.

5. **Monitor and Update Policies**:
   - Ensure DC Agents periodically check for and download new password policies.
   - Validate that policies are correctly applied during password changes.

6. **Test and Validate Deployment**:
   - Conduct tests in a subset of DCs before full deployment.
   - Verify consistent password policy enforcement across all DCs.

#### Notes
- Use a single distinguished Microsoft Entra tenant for each AD DS forest.
- Full deployment ensures security and consistent behavior.
- Partial deployment is suitable only for testing purposes.
## Implement single sign-on (SSO)

#### Key Considerations

1. **Administrative Roles**
   - Use minimal necessary permissions.
   - Assign roles: Help Desk Admin (view logs), Identity Admin (configure/debug), Application Admin (user attestation), Infrastructure Admin (manage certificates), Business Owner (user attestation).

2. **Certificate Management**
   - Default SAML certificates last three years; customize if needed.
   - Document expiration dates and set renewal processes.
   - Assign roles for certificate updates and monitoring.

3. **Communication**
   - Inform users about changes, timing, and support options.
   - Implement a clear communication plan before and after changes.

4. **Licensing**
   - Ensure proper Microsoft Entra and application licenses.
   - Align user roles in Microsoft Entra with application licenses.

5. **Shared Accounts**
   - Document user sets and credentials.
   - Create security groups and manage credentials in Microsoft Entra.
   - Set automatic password rollover if supported.

6. **SSO Options**
   - **Cloud Applications**: OpenID Connect, OAuth, SAML, password-based, linked SSO.
   - **On-Premises Applications**: Password-based, Integrated Windows Authentication (IWA), header-based, linked SSO.
	   - The Linked option lets you configure the target location when a user selects the application in your organization's end user portals.
   - **Protocols**: Use OpenID Connect/OAuth if available, SAML for existing apps, password-based for HTML sign-ins and shared accounts, linked for external identity providers, IWA for Windows authentication, and header-based for header authentication.

#### Deployment Steps

1. **Plan Roles**
   - Assign minimal necessary roles.
2. **Manage Certificates**
   - Verify and document certificate expirations, set renewal processes.
3. **Communicate**
   - Notify users of changes and provide support details.
4. **Verify Licensing**
   - Ensure all necessary licenses are in place.
5. **Handle Shared Accounts**
   - Document and manage credentials securely.
6. **Configure SSO**
   - Select and configure appropriate SSO methods.
## Integrate single sign on (SSO) and identity providers

#### Integration Steps

1. **Select SaaS Applications**
   - Choose from pre-integrated apps listed in Microsoft Entra ID Marketplace.
   - Request SCIM-enabled or SAML/OIDC-enabled apps for automatic provisioning or SSO.

2. **Cloud SaaS Providers**
   - Atlassian Cloud, ServiceNow, Slack, SuccessFactors, Workday, AWS Console, Alibaba Cloud, Google Cloud Platform, Salesforce, SAP Cloud Identity Platform.

3. **Example: Integrating Slack**
   - Manage access control and automatic sign-in with Microsoft Entra ID.
   - Control accounts centrally.

#### Prerequisites

1. **Microsoft Entra Subscription**
   - Free or paid account required.
2. **Slack SSO-Enabled Subscription**
   - Ensure SSO is enabled for Slack.

#### Adding Slack from Gallery

1. **Access Microsoft Entra Admin Center**
   - Sign in as Cloud Application Administrator.
2. **Add Slack**
   - Navigate to Identity > Applications > Enterprise applications > New application.
   - Search for Slack, add the app, and wait for it to be added.

#### Configuring and Testing SSO for Slack

1. **Configure Microsoft Entra SSO**
   - Sign in as Cloud Application Administrator.
   - Navigate to Slack > Single sign-on > SAML.
   - Edit Basic SAML Configuration and configure required fields.
   - Download the SAML Signing Certificate.

2. **Create Microsoft Entra Test User**
   - Sign in as User Administrator.
   - Create a test user named B.Simon.

3. **Assign Microsoft Entra Test User**
   - Assign access to Slack for B.Simon.

4. **Configure Slack SSO**
   - Sign in to Slack as an administrator.
   - Configure SAML authentication with values from Microsoft Entra.

5. **Create Slack Test User**
   - Slack supports just-in-time provisioning.

6. **Test SSO**
   - Initiate login flow from Slack Sign-on URL.
## Recommend and enforce modern authentication protocols

#### Microsoft's Recommendations

1. **Passwordless Authentication**
   - Windows Hello, FIDO2 security keys, Microsoft Authenticator app.
   - Offers the most secure sign-in experience.

2. **Multifactor Authentication (MFA)**
   - Adds extra security layers beyond passwords.
   - Users can authenticate via push notifications, tokens, or phone calls.

#### Simplifying User Onboarding

1. **Combined Security Information Registration**
   - Simplifies onboarding for MFA and self-service password reset (SSPR).
   - Requires users to register multiple authentication methods for resilience.

#### Authentication Method Strength and Security

1. **Security Considerations**
   - Choose methods meeting security, usability, and availability requirements.
   - Prefer methods with high security levels.

#### Authentication Methods Overview

| Authentication Method       | Security | Usability | Availability |
|------------------------------|----------|-----------|--------------|
| Windows Hello for Business  | High     | High      | High         |
| Microsoft Authenticator     | High     | High      | High         |
| Authenticator Lite          | High     | High      | High         |
| FIDO2 security key          | High     | High      | High         |
| Certificate-based           | High     | High      | High         |
| OATH hardware tokens (preview) | Medium | Medium    | High         |
| OATH software tokens        | Medium   | Medium    | High         |
| Temporary Access Pass (TAP) | Medium   | High      | High         |
| SMS                          | Medium   | High      | Medium       |
| Voice                        | Medium   | Medium    | Medium       |
| Password                     | Low      | High      | High         |
# Manage Microsoft Entra authorization

## Configure Azure role permissions for management groups, subscriptions, resource groups, and resources

#### Purpose:
- Efficiently manage access, policies, and compliance for multiple subscriptions.
- Organize subscriptions into containers called "management groups".
- Apply governance conditions to management groups, automatically inherited by subscriptions within.

#### Key Tasks:
1. **Change Management Group Name:**
   - Portal: Navigate to Management Groups > Select Group > Rename.
   - PowerShell: Use `Update-AzManagementGroup`.
   - Azure CLI: Utilize the `az account management-group update` command.

2. **Delete Management Group:**
   - Ensure no child groups or subscriptions exist.
   - Requires write permissions on the management group.
   - Portal: Select Group > Delete.
   - PowerShell: Employ `Remove-AzManagementGroup`.
   - Azure CLI: Use `az account management-group delete`.

3. **View Management Groups:**
   - Portal: Explore Management Groups.
   - PowerShell: Utilize `Get-AzManagementGroup`.
   - Azure CLI: Use `az account management-group list`.

4. **Moving Management Groups and Subscriptions:**
   - Ensure permissions and requirements are met.
   - Portal: Add/Remove subscriptions to/from a group.
   - PowerShell: Use `New-AzManagementGroupSubscription` and `Remove-AzManagementGroupSubscription`.
   - Azure CLI: Utilize `az account management-group subscription add` and `az account management-group subscription remove`.

5. **Audit Management Groups Using Activity Logs:**
   - Supported in Azure Activity Log.
   - Query events related to management groups.
   - Scope path: `/providers/Microsoft.Management/managementGroups/{yourMgID}`.

6. **Referencing Management Groups from Other Resource Providers:**
   - Use `/providers/Microsoft.Management/managementGroups/{yourMgID}` as the scope path.
   - Example: Assigning a role using PowerShell: `New-AzRoleAssignment -Scope "/providers/Microsoft.Management/managementGroups/Contoso"`.

#### Important Summarized Notes:
- **Purpose**: Efficient management of access, policies, and compliance for multiple Azure subscriptions through management groups.
- **Tasks**: Change Name, Delete, View, Move Groups/Subscriptions, Audit using Activity Logs, and Reference from other Resource Providers.
- **Permissions**: Ensure appropriate permissions are met for renaming, deleting, and moving groups/subscriptions.
- **Scope Path**: Use `/providers/Microsoft.Management/managementGroups/{yourMgID}` when referencing management groups from other Resource Providers.
- **Efficiency**: Utilize Azure Portal, PowerShell, or Azure CLI based on preference and task requirements.
## Assign Microsoft Entra built-in roles
#### Purpose:

The Microsoft Entra built-in roles provide granular access control over various aspects of managing Microsoft Entra resources. These roles enable administrators to assign permissions to users based on their specific responsibilities, ensuring secure and efficient resource management within the Microsoft Entra environment. By assigning appropriate roles, organizations can maintain regulatory compliance, mitigate security risks, and streamline administrative tasks.

**Important Background Notes:**

1. **Application Administrator [PRIVILEGED]:**
   - Users in this role can create and manage enterprise applications, application registrations, and application proxy settings.
   - They can consent for delegated permissions and application permissions, except for Microsoft Graph application permissions.
   - This role enables managing application credentials and impersonating application identities, potentially elevating privileges.
   - Actions include managing admin consent request policies, creating/deleting applications, updating authentication, and more.

2. **Application Developer [PRIVILEGED]:**
   - Users can create application registrations when self-registration is disabled.
   - Allows consenting on one's behalf when users cannot consent to apps accessing company data.
   - Actions include creating applications, OAuth 2.0 permission grants, and service principals.

3. **Attribute Assignment Administrator:**
   - Enables assigning and removing custom security attribute keys and values for supported Microsoft Entra objects.
   - Actions include managing attribute sets, custom security attribute definitions, and reading/updating attribute values.

4. **Attribute Assignment Reader:**
   - Allows reading custom security attribute keys and values for supported Microsoft Entra objects.
   - Actions include reading attribute sets, custom security attribute definitions, and attribute values.

5. **Attribute Definition Administrator:**
   - Facilitates defining custom security attributes and activating/deactivating them.
   - Actions include managing attribute sets and custom security attribute definitions.

6. **Attribute Definition Reader:**
   - Permits reading the definition of custom security attributes.
   - Actions include reading attribute sets and custom security attribute definitions.

7. **Attribute Log Administrator/Reader:**
   - Administrators can read audit logs for custom security attribute value changes.
   - Allows configuring diagnostic settings for custom security attributes.

8. **Authentication Administrator [PRIVILEGED]:**
   - Users can set/reset authentication methods for non-administrators and perform sensitive actions.
   - Enables creating/managing Azure support tickets and managing user authentication.
   - Actions include updating authentication methods, deleting users, and managing service health.

9. **Authentication Policy Administrator:**
   - Allows configuring authentication methods policy, MFA settings, and password protection policy.
   - Permits creating and managing verifiable credentials and Azure support tickets.
#### Assigning Roles
1. **Select Role:**
   - Access Entra admin center.
   - Navigate to Users > All users.
   - Choose user > Assigned roles > Add assignments.
   - Select role from dropdown.

2. **Adjust Settings:**
   - Define role as eligible or active.
   - Choose Assignment type: Permanently eligible or specify date range.
   - Click Assign.

3. **Update Roles:**
   - Navigate to Users > All users.
   - Select user > Assigned roles.
   - Click Update for desired role.
   - Make changes > Save.

4. **Remove Roles:**
   - Go to Users > All users.
   - Choose user > Assigned roles.
   - Click Remove for role to delete.
   - Confirm action.
## Assign Azure built-in roles
**Azure Built-in Roles:**

1. **General:**
   - Contributor: Full resource management, no role assignment.
   - Owner: Full resource management, role assignment included.
   - Reader: View resources, no changes allowed.
   - Role Based Access Control Administrator: Manage access using Azure RBAC.

2. **Compute:**
   - Classic Virtual Machine Contributor: Manage classic VMs.
   - Data Operator for Managed Disks: Manage managed disks data.
   - Virtual Machine Contributor: Create, manage VMs, but no role assignment.

3. **Networking:**
   - Azure Front Door Domain Contributor: Manage Front Door domains.
   - DNS Zone Contributor: Manage DNS zones.
   - Network Contributor: Manage networks.

4. **Storage:**
   - Backup Contributor: Manage backup service.
   - Storage Account Contributor: Manage storage accounts.
   - Storage Blob Data Contributor: Read, write, delete blobs.

5. **Containers:**
   - AcrPull: Pull artifacts from a container registry.
   - Azure Arc Kubernetes Cluster Admin: Manage all resources in the cluster.

6. **Databases:**
   - Cosmos DB Account Reader Role: Read Azure Cosmos DB account data.
   - SQL DB Contributor: Manage SQL databases.

7. **Security:**
   - App Compliance Automation Administrator: Manage compliance automation.
   - Key Vault Contributor: Manage key vaults, no role assignment.

8. **Management and Governance:**
   - Automation Contributor: Manage Azure Automation resources.
   - Management Group Contributor: Manage management groups.
#### Assigning Roles
1. **Identify Scope:**
   - Sign in to Azure portal.
   - Search for Management groups, Subscriptions, Resource groups, or resources.
   - Click the specific resource.

2. **Open Add Role Assignment:**
   - Click Access control (IAM).
   - Click Role assignments tab.
   - Click Add > Add role assignment.

3. **Select Role:**
   - Choose a role from the Role tab.
   - Optionally, select Privileged administrator roles tab for admin roles.
   - Click Next.

4. **Select Users/Groups:**
   - On the Members tab, select User, group, or service principal.
   - Click Select members.
   - Find and select the users/groups.
   - Click Select.
   - Optionally, select Managed identity and choose managed identities.

5. **Add Description (Optional):**
   - Enter a description for the role assignment.
   - Click Next.

6. **Add Condition (Optional):**
   - If applicable, add conditions for the role assignment.
   - Click Select roles and principals.
   - Follow the steps in Delegate Azure role assignment management to others with conditions.

7. **Assign Role:**
   - Review role assignment settings on Review + assign tab.
   - Click Review + assign to assign the role.
## Create and assign custom roles, including Azure roles and Microsoft Entra roles

1. **Create a Custom Role:**
   - Sign in to Microsoft Entra admin center as a Privileged Role Administrator.
   - Navigate to Identity > Roles & admins > Roles & admins.
   - Click on New custom role.
   - On the Basics tab, provide a name and description for the role.
   - Click Next.
   - On the Permissions tab, select necessary permissions, like:
     - Search and select `microsoft.directory/applications/credentials/update`.
     - Search and select `microsoft.directory/applications/basic/update`.
   - Click Next.
   - Review permissions on the Review + create tab.
   - Select Create.

2. **Assign Custom Role Scoped to a Resource:**
   - Sign in to Microsoft Entra admin center as an Application Developer.
   - Navigate to Identity > Applications > App registrations.
   - Select the specific app registration.
   - Go to Roles and administrators.
   - Select the custom role to open the Assignments page.
   - Click Add assignment to add a user.
## Implement and manage Microsoft Entra Permissions Management

1. **Enable Permissions Management:**
   - Sign in to Microsoft Entra admin center as a Billing Administrator.
   - If needed, activate the Permissions Management Administrator role in your Microsoft Entra tenant.
   - In the Azure portal, select Microsoft Entra Permissions Management and proceed to purchase a license or start a trial.

2. **Activate a Free Trial or Paid License:**
   - You can activate a trial or license in two ways:
     - Via Microsoft 365 admin center:
       - Sign in as a Global Administrator.
       - Navigate to Setup and sign up for a Microsoft Entra Permissions Management trial.
     - Via Microsoft 365 portal:
       - Visit the portal to sign up for a 45-day free trial or purchase licenses.
     - For Volume Licensing or Enterprise agreements:
       - Contact your Microsoft representative.

3. **Configure Data Collection Settings:**
   - Use the Data Collectors dashboard in Permissions Management to configure data collection settings for your authorization system.
   - If the dashboard isn't displayed initially:
     - Go to the Permissions Management home page.
     - Select Settings (gear icon), then choose the Data Collectors subtab.
   - Select the authorization system you want to configure: AWS, Azure, or GCP.

4. **Onboard Authorization Systems:**
   - Follow specific instructions to onboard an AWS account, Azure subscription, or GCP project into Permissions Management.
   - For detailed guidance, select one of the following articles:
     - Onboard an AWS account
     - Onboard an Azure subscription
     - Onboard a GCP project
## Configure Microsoft Entra Privileged Identity Management
#### Background Information
**Introduction:**
Privileged Identity Management (PIM) in Microsoft Entra ID allows you to manage, control, and monitor access to critical resources in your organization, including those in Microsoft Entra ID, Azure, and other Microsoft Online Services like Microsoft 365 or Intune. Here's how to configure it effectively.

**Reasons to Use:**
- Minimize the risk of unauthorized access or misuse of resources.
- Provide just-in-time privileged access to resources.
- Enable time-based and approval-based role activation.
- Ensure multifactor authentication and conditional access enforcement.
- Facilitate access reviews and audit history monitoring.

**Understanding PIM:**
- PIM supports Microsoft Entra roles, Azure roles, and PIM for Groups.
- Users or groups can be assigned eligible or active roles.
- The principle of least privilege guides role assignment.
- There are four types of assignments: permanent eligible, permanent active, time-bound eligible, and time-bound active.
  
**Project Planning:**
- Engage stakeholders and define roles clearly.
- Plan a pilot with a small user group to validate functionality.
- Communicate changes and provide support documentation.
- Plan testing thoroughly, including rollback strategies.

**Configuring PIM for Microsoft Entra Roles:**
- Discover and mitigate privileged roles.
- Prioritize roles to be managed by PIM, starting with Global and Security admin roles.
- Configure PIM settings for each role, specifying MFA, approval requirements, activation duration, etc.
- Assign and activate Microsoft Entra roles, ensuring only authorized administrators can manage assignments.
- Approve or deny activation requests and view audit history for role assignments.

**Configuring PIM for Azure Resource Roles:**
- Discover and mitigate privileged roles, focusing on minimizing Owner and User Access Administrator assignments.
- Determine roles to be managed by PIM, prioritizing critical subscriptions or resources.
- Configure PIM settings for Azure Resource roles, specifying MFA, notification, approval requirements, etc.
- Assign eligibility for PIM for Groups, ensuring group owners approve activation requests.
- Approve or deny activation requests for PIM for Groups and view audit history.

##### Implementing
1. **Introduction:**
   - Privileged Identity Management (PIM) offers time-based and approval-based role activation to mitigate risks associated with excessive access permissions.
   - Key features include just-in-time privileged access, time-bound access with start and end dates, approval requirements, multifactor authentication enforcement, justification recording, notification alerts, access reviews, and audit history download.

2. **Navigation:**
   - Once activated, you'll find Tasks, Manage, and Activity options in the left navigation menu.
   - Administrators can select options such as managing Microsoft Entra roles, Azure resource roles, or PIM for Groups, each presenting relevant management options.

3. **Role Assignment Overview:**
   - PIM facilitates secure access to organizational resources via role assignments, including assigning roles, activating assignments, approving or denying requests, and managing assignment duration.
   - Email notifications keep participants informed about role assignment updates and relevant tasks.

4. **Assign:**
   - Administrators initiate the assignment process by assigning roles to members, specifying:
     - Members or owners.
     - Assignment scope.
     - Assignment type (eligible or active).
     - Duration (start and end dates or permanent).

5. **Activate:**
   - Users activate assigned roles if eligible, selecting activation duration and providing reasons for activation.
   - If approval is required, pending requests are displayed, and members can start using the role upon approval.

6. **Approve or Deny:**
   - Delegated approvers receive notifications for pending role requests and can view, approve, or deny them.
   - Approved requests allow members to start using the assigned roles.

7. **Extend and Renew Assignments:**
   - Users can request extension or renewal of expired role assignments through PIM, requiring approval from a Global Administrator or Privileged Role Administrator.
   - Admins can easily manage extension or renewal requests with simple approval or denial.

8. **Scenarios Supported:**
   - Privileged Role Administrator permissions.
   - Enabling approval for specific roles.
   - Specifying approver users or groups.
   - Viewing request and approval history.
   - Approver permissions to view and approve/deny requests.
   - Providing justification for approvals or rejections.
   - Eligible role user permissions to request activation and view request status.
   - Utilizing Microsoft Graph APIs for programmatically managing PIM.
## Configure role management and access reviews in Microsoft Entra

**Introduction:**
Regularly reviewing and managing access to privileged Azure resources and Microsoft Entra roles is crucial to reduce security risks associated with stale role assignments. Microsoft Entra Privileged Identity Management (PIM) offers tools to create access reviews for these roles and automate the review process.

**Prerequisites:**
- Licensing requirements for Privileged Identity Management.
- Assignments to specific roles (Owner, User Access Administrator, Global Administrator, or Privileged Role Administrator) based on the type of access review.

**Types of Licenses:**
- Free: Included with Microsoft cloud subscriptions.
- Microsoft Entra ID P1: Available standalone or included in Microsoft 365 E3 or Business Premium.
- Microsoft Entra ID P2: Available standalone or included in Microsoft 365 E5.
- Microsoft Entra ID Governance: Offers advanced identity governance capabilities for P1 and P2 customers.

**Create Access Reviews:**
1. Sign in to the Microsoft Entra admin center with appropriate role assignments.
2. Navigate to Identity governance > Privileged Identity Management.
3. Choose either Microsoft Entra roles or Azure resources.
4. Select the desired role or resource.
5. Under Manage, choose Access reviews, then New to create a new access review.
6. Name the review, set start and end dates, and configure recurrence if needed.
7. Define the scope of the review, including users, groups, or service principals.
8. Select the roles to review and specify the assignment type.
9. Choose reviewers (individuals or groups) and define their role in the review process.
  
**Upon Completion Settings:**
- Enable Auto apply results to resource to automatically remove access for denied users.
- Choose actions for users not reviewed within the specified period.
- Notify additional users or groups about review completion updates.

**Advanced Settings:**
- Show recommendations to reviewers based on user access information.
- Require reason on approval for reviewer justification.
- Enable mail notifications and reminders for reviewers.
- Customize email content with additional instructions or contact information.
## Implement Conditional Access policies
**Introduction:**
The modern security landscape emphasizes identity-driven access control, extending beyond traditional network perimeters. Microsoft Entra Conditional Access provides a policy engine to make access decisions based on various signals, ensuring secure access to organizational resources.

**Goals:**
Administrators aim to:
1. Empower users for productivity.
2. Safeguard organizational assets.

**Conditional Access Policies:**
- Function as if-then statements, dictating access requirements based on user actions.
- Example: A payroll manager accessing a payroll application must undergo multifactor authentication.

**Common Signals:**
Conditional Access considers multiple signals for access decisions:
1. **User or Group Membership:**
   - Grants fine-grained access control to specific users or groups.
2. **IP Location Information:**
   - Defines trusted IP address ranges for policy decisions.
   - Allows blocking or allowing traffic from entire countries/regions.
3. **Device:**
   - Considers device platform or state for policy enforcement.
   - Filters enable targeting policies to privileged access workstations.
4. **Application:**
   - Triggers different policies based on the application being accessed.
5. **Real-time and Calculated Risk Detection:**
   - Integrates with Microsoft Entra ID Protection for identifying and remediating risky sign-in behavior.
6. **Microsoft Defender for Cloud Apps:**
   - Monitors and controls user application access and sessions in real-time, enhancing visibility and control over cloud environment activities.

**Common Decisions:**
- **Block Access:**
  - Most restrictive decision.
- **Grant Access:**
  - Least restrictive decision, often with additional requirements like multifactor authentication or device compliance.

**Commonly Applied Policies:**
- Multifactor authentication for administrative roles or Azure management tasks.
- Blocking sign-ins for legacy authentication protocols.
- Requiring trusted locations for multifactor authentication registration.
- Blocking or granting access based on location.
- Managing risky sign-in behaviors.
- Mandating organization-managed devices for specific applications.
# Manage Microsoft Entra application access

## Manage access to enterprise applications in Microsoft Entra ID, including OAuth permission grants
### Manage Access to Enterprise Applications in Microsoft Entra ID, including OAuth Permission Grants

#### Overview
- **Challenge**: Ongoing access management, usage evaluation, and reporting post-integration.
- **Solution**: Microsoft Entra ID simplifies access management and reporting for configured applications.

#### Key Features

1. **User and Group Assignment**
   - **Individual Assignment**: 
     - Admins (Global Administrator role) assign specific users to apps.
   - **Group-Based Assignment** (Requires Entra ID P1 or P2):
     - Admins assign groups, leveraging dynamic, external system, or self-service groups.
     - Facilitates shared assignment rules across multiple apps.

2. **Requiring User Assignment**
   - **Supported Applications**: 
     - Federated SSO (SAML), Application Proxy with Pre-Authentication, OAuth 2.0/OpenID Connect apps.
   - **User Assignment Requirement**:
     - Ensures only assigned users can access the app.
     - Unassigned users can still access via direct links or IDP-initiated sign-on if assignment isn’t required.

3. **User Experience Customization**
   - Deploy applications via My Apps, Microsoft 365 launcher, or direct sign-on links.
   - Control visibility of apps in My Apps and Microsoft 365 launcher.

#### Example: Salesforce Application Management
- **Scenario**:
  - Different roles for marketing and sales teams, with exceptions managed by team leaders.
- **Implementation**:
  - Use dynamic groups for automated role assignments.
  - Create self-service groups for exceptions, managed by leadership.
  - Automatic provisioning and role updates in Salesforce based on group membership.

#### Access Policies with Conditional Access
- Set specific policies for app roles, including location-based access, multifactor authentication, and device compliance.

#### Access to Microsoft Applications
- **Methods**:
  - License assignment for paid suites.
  - User consent for free apps.
  - Administrator consent for organization-wide access.
- **Access Points**:
  - Office 365 portals.
  - Controlled visibility in My Apps via admin settings.
## Manage Microsoft Entra app registrations

#### Overview
- Registering a new application in Microsoft Entra ID creates a service principal.
- The service principal controls access to resources via assigned roles.
- For enhanced security, use managed identities for Azure resources.

#### Prerequisites
- **Account**: A Microsoft Entra user account (free to create).
- **Permissions**: Application.ReadWrite.All permission to register and assign roles.

#### Steps to Register an Application

1. **Sign In**: Use a Cloud Application Administrator account in Microsoft Entra admin center.
2. **Navigate to Registration**:
   - Go to Identity > Applications > App registrations.
   - Click on New registration.
3. **Configure Application**:
   - Name the app (e.g., "example-app").
   - Select account type (who can use the app).
   - Specify Redirect URI (Web).
   - Click Register.

#### Assigning Roles to the Application

1. **Sign In**: Use the Azure portal.
2. **Select Scope**:
   - Choose the level (subscription, resource group, resource).
   - Navigate to Subscriptions if assigning at the subscription level.
3. **Assign Role**:
   - Go to Access control (IAM) > Add role assignment.
   - Select the role (e.g., Contributor for managing resources).
   - Choose the application by searching its name.
   - Click Review + assign.

#### Authentication Setup

1. **Obtain IDs**:
   - Go to Identity > Applications > App registrations.
   - Copy the Directory (tenant) ID and Application (client) ID.

2. **Authentication Methods**:
   - **Certificates (Recommended)**:
     - Upload a trusted certificate via Certificates & secrets > Certificates > Upload certificate.
   - **Self-Signed Certificate (Testing)**:
     - Create with PowerShell: 
       ```powershell
       $cert = New-SelfSignedCertificate -Subject "CN=DaemonConsoleCert" -CertStoreLocation "Cert:\CurrentUser\My" -KeyExportPolicy Exportable -KeySpec Signature
       ```
     - Export and upload the certificate.
   - **Client Secret**:
     - Generate a new secret via Certificates & secrets > Client secrets > New client secret.
     - Save the secret value securely.

#### Configuring Access Policies

1. **Sign In**: Use the Azure portal.
2. **Key Vault Access**:
   - Select the key vault.
   - Go to Access policies > Add access policy.
   - Assign key, secret, and certificate permissions to the service principal.
   - Save changes.
## Configure app registration permission scopes

#### Overview
Register your web API, assign owners, define app roles, and configure permission scopes to control access for users and client applications.

#### Register the Web API
1. **Register Application**:
   - Follow steps in Quickstart: Register an app with the Microsoft identity platform.
   - Skip configuring Redirect URI for a web API.
   
#### Assign Application Owner
1. **Set Owners**:
   - Go to your app registration.
   - Under **Manage**, select **Owners** > **Add owners**.
   - Select owners and confirm.
   - Ensure both API and client applications have an owner for API listing.

#### Assign App Role
1. **Create App Role**:
   - Under **Manage**, select **App roles** > **Create app role**.
   - Fill in role attributes:
     - **Display name**: e.g., Employee Records
     - **Allowed member types**: e.g., Applications
     - **Value**: e.g., Employee.Records
     - **Description**: Detailed description
   - Enable the app role.

#### Add a Scope
1. **Sign In**:
   - Use Microsoft Entra admin center as Cloud Application Administrator.
2. **Navigate to API Registration**:
   - Go to Identity > Applications > App registrations.
   - Select your API’s app registration.
3. **Expose an API**:
   - Select **Add** next to Application ID URI.
   - Use default value: api://\<application-client-id>.
4. **Add Scope**:
   - Select **Add a scope**.
   - Fill in scope attributes:
     - **Scope name**: e.g., Employees.Read.All
     - **Who can consent**: e.g., Admins and users
     - **Admin consent display name**: e.g., Read-only access to Employee records
     - **Admin consent description**: e.g., Allow read-only access to all Employee data
     - **User consent display name**: e.g., Read-only access to your Employee records
     - **User consent description**: e.g., Allow read-only access to your Employee data
   - Enable and add the scope.

#### Pre-Authorize Client Applications (Optional)
1. **Suppress Consent Prompting**:
   - Under **Authorized client applications**, select **Add a client application**.
   - Enter the client application's ID.
   - Select the scopes to pre-authorize.
   - Add the application.

#### Add Admin Consent Scope
1. **Create Scope**:
   - Follow steps to add a scope.
   - Use the following values:
     - **Scope name**: Employees.Write.All
     - **Who can consent**: Admins only
     - **Admin consent display name**: Write access to Employee records
     - **Admin consent description**: Allow write access to all Employee data
     - Leave user consent fields empty.
   - Enable and add the scope.

#### Verify Exposed Scopes
1. **Check Scopes**:
   - Go to **Expose an API** pane.
   - Ensure the added scopes (e.g., Employees.Read.All and Employees.Write.All) appear with correct URIs.

## Manage app registration permission consent
#### Overview
To access protected resources, your app needs authorization from the resource owner. This can be done via delegated access (user-signed in) or app-only access (no user).

#### Access Scenarios
1. **Delegated Access**:
   - User signs in, and the app acts on their behalf.
   - Requires delegated permissions (scopes).
   - Example: An app accessing a user’s email.

2. **App-Only Access**:
   - The app acts independently.
   - Requires app roles (application permissions).
   - Example: A backup service accessing files.

#### Types of Permissions
1. **Delegated Permissions**:
   - Act on a user’s behalf.
   - Access only what the user can access.
   
2. **Application Permissions**:
   - Act independently.
   - Access all data associated with the permission.
   - Only admins can consent.

#### Consent Process
1. **User Consent**:
   - User signs in and sees a consent prompt if needed.
   
2. **Admin Consent**:
   - Required for higher-privilege permissions.
   - Admins can consent for the organization.

3. **Preauthorization**:
   - Resource owners grant permissions to avoid user consent prompts.
   - Set up via Azure portal, PowerShell, or Microsoft Graph.
## Manage and use service principals
#### Overview
Your app needs authorization from resource owners to access protected resources, either through delegated access (user-signed in) or app-only access (no user).

#### Access Scenarios
1. **Delegated Access**:
   - App acts on behalf of a signed-in user.
   - Requires delegated permissions (scopes).
   - Example: Accessing a user’s email.

2. **App-Only Access**:
   - App acts independently.
   - Requires app roles (application permissions).
   - Example: Backup service accessing files.

#### Types of Permissions
1. **Delegated Permissions**:
   - Act on a user’s behalf.
   - Access only what the user can access.
   
2. **Application Permissions**:
   - Act independently.
   - Access all data associated with the permission.
   - Only admins can consent.

#### Consent Process
1. **User Consent**:
   - User sees a consent prompt when signing in if needed.
   
2. **Admin Consent**:
   - Required for higher-privilege permissions.
   - Admins can consent for the organization.

3. **Preauthorization**:
   - Resource owners grant permissions to avoid user consent prompts.
   - Set up via Azure portal, PowerShell, or Microsoft Graph.
## Manage managed identities for Azure resources
#### Overview
Managed identities simplify secure communication between services by eliminating the need for managing secrets, credentials, and keys.

#### Benefits
- No credential management required.
- Authenticate to any Microsoft Entra-authenticated resource.
- Free to use.

#### Types of Managed Identities
1. **System-Assigned**:
   - Enabled directly on an Azure resource.
   - Tied to the resource's lifecycle.
   - Deleted automatically with the resource.
   - Example: A VM with an identity tied to its lifecycle.

2. **User-Assigned**:
   - Created as a standalone resource.
   - Can be assigned to multiple Azure resources.
   - Independently managed.
   - Example: Multiple VMs sharing a single identity.

#### Comparison

| Property                         | System-Assigned Managed Identity                 | User-Assigned Managed Identity                      |
|----------------------------------|--------------------------------------------------|----------------------------------------------------|
| **Creation**                     | Part of an Azure resource                        | Standalone Azure resource                           |
| **Lifecycle**                    | Tied to parent resource                          | Independent                                        |
| **Sharing**                      | Not shareable                                    | Shareable across multiple resources                |
| **Use Cases**                    | Single resource workloads                        | Multiple resource workloads needing a single identity |

#### Usage
1. **Create a Managed Identity**:
   - Choose system-assigned or user-assigned.
   - For user-assigned, assign it to the source resource.

2. **Authorize Access**:
   - Authorize the identity to access the target service.

3. **Access Resources**:
   - Use Azure SDK with Azure.Identity library.
   - Source resources may offer connectors for managed identities.

#### Supported Services
Managed identities can authenticate to any service supporting Microsoft Entra authentication.

#### Operations
- **System-Assigned**:
  - Enable/disable at the resource level.
  - Use RBAC to grant permissions.
  - Review CRUD operations and sign-in activities in logs.

- **User-Assigned**:
  - Create, read, update, delete identities.
  - Assign RBAC roles for permissions.
  - Use across multiple resources.
  - Review CRUD operations and sign-in activities in logs.

#### Tools for Management
- Azure Resource Manager templates
- Azure portal
- Azure CLI
- PowerShell
- REST APIs
## Recommend when to use and configure an Microsoft Entra Application Proxy, including authentication
#### Overview
- **Purpose**: Secure, cost-effective remote access to on-premises applications.
- **Use Case**: Suitable for remote users needing access to internal resources; replaces the need for VPN or reverse proxy.

#### When to Use
- **Remote Access**: Ideal for remote users accessing internal resources.
- **Not for Intranet**: Not recommended for users on the corporate network due to potential performance issues.

### Implementation Planning

#### Prerequisites
- **Connectors**: 
  - Deploy on physical hardware, VMs (hypervisor solutions), or Azure VMs.
  - Enable TLS 1.2.
  - Deploy connectors in the same network segment as back-end servers.
  - Use at least two connectors per group for high availability; three is optimal.

- **Network Access Settings**:
  - Connect via HTTPS (TCP 443) and HTTP (TCP 80).
  - No inline inspection on outbound TLS communications.
  - Load balancing not supported for connectors.

#### Core Requirements
- **Azure Onboarding**: Synchronize user identities from on-premises or create directly in Microsoft Entra tenants.
- **Conditional Access**: Recommended for remote access with pre-authentication.
- **Service Limits**: Be aware of throttling limits.
- **Public Certificate**: Necessary for custom domains.
- **Domain Requirements**: Servers must be domain-joined for Kerberos Constrained Delegation (KCD).

#### DNS Records
- Create CNAME records for custom domains.
- Connector hosts must resolve internal application URLs.

#### Administrative Rights and Roles
- **Installation**: Requires local admin rights.
- **Publishing**: Requires Application Administrator role.

#### Licensing
- Requires Microsoft Entra ID P1 or P2 subscription.

### Application Discovery
- Collect detailed information about each application (service type, platform, domain, location, internal/external access URLs, public certificate, authentication type, etc.).

### Organizational Requirements
- **Access**: Define access requirements for domain-joined devices, personal devices, etc.
- **Governance**: Monitor user assignments.
- **Security**: Ensure access is restricted to assigned users.
- **Performance**: No performance degradation compared to internal network access.
- **User Experience**: Use familiar URLs.
- **Auditing**: Ability to audit user access.

### Best Practices for Pilot Implementation
- **Connector Management**: Use Default connector group initially.
- **Application Management**: Use familiar top-level verified domains.
- **Single Sign-On (SSO)**: Ensure dependencies are addressed ahead of time.
- **TLS**: Always use TLS between connector host and target applications.

### Publishing Applications
- **Use Connector Groups**: Assign appropriate groups for high availability and segmentation.
- **Backend Application Timeout**: Set to Long (180 seconds) if necessary.
- **Cookie Types**: Use HTTP-Only, Secure, and Persistent cookies as needed.
- **Translate URLs**: Enable translation in headers and application body if required.

### Managing Access
- **User Assignment**: Use on-premises groups, Dynamic Groups, self-service groups, or a combination.
- **Self-Service Access**: Allow users to request access via MyApps portal.
- **Guest Users**: Invite through Microsoft Entra B2B.
- **Anonymous Access**: Use cautiously for on-premises applications.

### Enable Pre-Authentication and Single Sign-On
- **Pre-Authentication**: Verify via external URL and configure Microsoft Entra ID.
- **Single Sign-On**: Configure SSO after pre-authentication.

### Working with Applications
- **Support for MSAL**: Support applications using Microsoft Authentication Library.
- **Conditional Access**: Implement user, location, device, application, and risk-based policies.

### Manage Your Implementation
- **Roles**: Assign the least privilege necessary.
- **Privileged Identity Management**: Use JIT policies for on-demand access.

### Reporting and Monitoring
- **Audit Logs**: Available in Microsoft Entra admin center and via Audit API.
- **Connector Monitoring**: Monitor from the admin center.
- **Windows Event Logs**: Include admin and session logs for troubleshooting.