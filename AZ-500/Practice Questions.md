Q: You have a workload in Azure that uses multiple virtual machines and Azure functions to access data in a storage account.

You need to ensure that all access to the storage account is done by using a single identity. The solution must reduce the overhead of managing the identity.

Which type of identity should you use?

A: user-assigned managed identity

A user assigned managed identity can be shared across Azure resources, and its password changes are handled by Azure. An user needs to manually handle password changes. You cannot use a group as a service principle. Multiple Azure resources cannot share system-assigned managed identities.

Q: You manage external guest users in a Microsoft Entra tenant. The tenant uses the default settings.

Which capability is available to the guest users?

A: Invite other guests.

By default, guest users can invite other guests. They are unable to read all directory information, register new applications, or read subscriptions.

Q: You have a Microsoft Entra tenant. All the users in the tenant have Windows devices that are Microsoft Entra-joined.

You need to implement Multi-Factor Authentication (MFA). The solution must ensure that Azure MFA can be used without internet access or mobile network availability.

Which authentication method should you use?

A: Windows Hello for Business

When you configure Microsoft Entra MFA, you can configure authentication to use Windows Hello for Business. With this method, users will sign in by using a biometric factor such as a fingerprint or require a PIN to be entered on the device. With Windows Hello for Business, validation is performed locally. Internet access or a mobile network are not needed nor required. The remaining options require a mobile network or a network connection to provide authentication.

Q: You have an Azure subscription.

You plan to deploy Microsoft Entra Verified ID.

You need to identify which administrative roles are required for the solution. The solution must follow the principle of least privilege.

Which three roles should you identify? Each correct answer presents part of the solution.

A: Application Administrator, Authentication Policy Administrator & Contributor

The Authentication Policy Administrator role can configure policies and create and manage verified credentials. The Application Administrator role is used to complete app registrations, including granting admin consent. The Contributor role is required to manage all the resources in the subscription. The Global Administrator role does not meet the requirements of least privilege. The User Administrator role only manages users. The Privileged Authentication Administrator role cannot create and manage verified credentials.

Q: You need to provide an administrator with the ability to configure access reviews in Microsoft Entra Privileged Identity Management (PIM). The solution must follow the principle of least privilege.

Which role should you assign to the administrator?

A: Privileged Role Administrator

Privileged role administrators can manage PIM. Assigning the Global Administrator role does not follow the principle of least privilege. Security administrators have permissions to manage security-related features. Privileged authentication administrators can set or reset any authentication method, including passwords, for any user, including global administrators.

Q: You manage Microsoft Entra tenant.

You disable the Users can register applications option in Microsoft Entra.

A user reports that they are unable to register an application.

You need to ensure that that the user can register applications. The solution must follow the principle of least privilege.

What should you do?

A: Assign the Application Developer role to the user.

The Application Developer role has permissions to register an application even if the Users can register applications option is disabled. The Users can register applications option allows any user to register an application. The Authentication Administrator role and the Cloud App Security Administrator role do not follow the principle of least privilege.

Q: You create a role by using the following JSON.


```json
{
  "Name": "Virtual Machine Operator",
  "Id": "88888888-8888-8888-8888-888888888888",
  "IsCustom": true,
  "Description": "Can monitor and restart virtual machines.",
  "Actions": [
    "Microsoft.Storage/*/read",
    "Microsoft.Network/*/read",
    "Microsoft.Compute/virtualMachines/start/action",
    "Microsoft.Compute/virtualMachines/restart/action",
    "Microsoft.Authorization/*/read",
    "Microsoft.ResourceHealth/availabilityStatuses/read",
    "Microsoft.Resources/subscriptions/resourceGroups/read",
    "Microsoft.Insights/alertRules/*",
    "Microsoft.Insights/diagnosticSettings/*",
    "Microsoft.Support/*"
  ],
  "NotActions": [],
  "DataActions": [],
  "NotDataActions": [],
  "AssignableScopes": ["/subscriptions/*"]
}
```

A user that is part of the new role reports that they are unable to restart a virtual machine by using a PowerShell script.

What should you do to ensure that the user can restart the virtual machine?

A: Add `Microsoft.Compute/*/read` to the list of `Actions` in the role.

The role needs read access to virtual machines to restart them. The user does not need to authenticate again for the role to be in effect, and the user will not be able to access the virtual machine from the portal. Adding `Microsoft.Compute/virtualMachines/login/action` to the list of `DataActions` in the role allows the user to sign in as a user, but not to restart the virtual machine.

Q: You have a Microsoft Entra tenant and an Amazon Web Services (AWS) account.

You plan to implement Microsoft Entra Permissions Management to manage the permissions granted to all the identities across Microsoft Entra ID and AWS.

You purchase the required license for Permissions Management.

You need to use Permissions Management to manage permissions in Microsoft Entra ID and AWS.

What should you configure first?

A: Data Collectors

To start using Entra Permission Management you first need to configure data collection from the Permission Management portal. This will enable you to onboard services such as Azure, AWS and Google Cloud. All other items can be configured after you enable data collection.

Q: You create a web API and register the API as a Microsoft Entra application.

You need to expose a function in the API to ensure that administrators must provide consent to apps that use the API.

What should you add to your app registration?

A: a scope

A scope is used to request content to run a given function in an API. An application ID URI does not handle permissions, a permission is used to allow an application to access the scope created in another app, and a client application allows an application to use the API.

Q: You create a Microsoft Entra app registration.

You need to consent to the use of a given API in your app for all users.

What should you add to your app registration?

A: a permission

A permission allows the application to use a given API. A scope is used to request consent to run a given function on an API. An application ID URI does not handle permissions.

Q: You are managing permission consent for Microsoft Entra app registration.

Which component displays the publisher domain?

A: publisher name and verification

The publisher displays more app info as it becomes available, including the publisher name, publisher domain, date created, certification details, and reply URLs. Publisher information, Microsoft 365 certification, and app name do not display publisher domain information.

Q: You are creating a Microsoft Entra app registration. You are configuring credentials for the app registration and have the following requirements:

- Ensure that the credentials are not transmitted during authentication.
- Ensure that the credentials are stored securely.
- Ensure that credential usage follows the principle of least privilege.

What should you do?

A: Use certificate credentials.

Using certificate credentials ensures that the credentials are not transmitted during authentication, that they are stored securely, and that the credential usage follows the principle of least privilege.

Q: You have Azure web apps named App1 and App2.

You need to ensure that App1 and App2 use the same identity.

Which identity type should you use?

A: a user-assigned managed identity

A user-assigned managed identity can be associated with more than one Azure resource. Creating a system-assigned managed identity cannot be pre-authorized. Creating a service principal with password-based authentication or certificate-based authentication involves the use of credentials.

Q: You have a Microsoft Entra tenant that uses the default setting.

You need to prevent users from a domain named contoso.com from being invited to the tenant.

What should you do?

A: Edit the Collaboration restrictions settings.

After you edit the Collaboration restrictions settings, if you try to invite a user from a blocked domain, you cannot. Security defaults and PIM do not affect guest invitation privileges. By default, the Allow invitations to be sent to any domain (most inclusive) setting is enabled. In this case, you can invite B2B users from any organization.

Q: You create an application named App1 in an Azure tenant.

You need to host the application as a multitenant application for any users in Azure, while restricting non-Azure accounts.

You need to allow administrators in other Azure tenants to add the application to their gallery.

Which CLI command should you run?

A: `az ad app create –display-name app1--sign-in-audience AzureADMultipleOrgs`

The correct CLI command allows the application to provide SSO for Microsoft Entra users in any tenant. The CLI commands requiring a web app do not create a gallery entry for the application and configuring the sign-in audience to Microsoft Entra and personal Microsoft accounts does not restrict users to only Azure accounts.

Q: You plan to provide connectivity between Azure and your company’s datacenter.

You need to define how to establish the connection. The solution must meet the following requirements:

- All traffic between the datacenter and Azure must be encrypted.
- Bandwidth must be between 10 and 100 Gbps.

What should you use for the connection?

A: ExpressRoute Direct

ExpressRoute Direct can have up to 100 Gbps and use MACSec for Layer 2 encryption. ExpressRoute with a provider does not allow for MACSec encryption and can only use up to 10 Gbps. VPN Gateway and VPN Gateway with Virtual WAN cannot support a bandwidth over 1 Gbps.

Q: You have an Azure virtual network named VNet1. VNet1 is in a resource group named RG1. VNet1 contains the following two subnets:

- Subnet1: 10.0.1.0/24
- Subnet2: 10.0.2.0/24

You need to configure access to a storage account named sa1 in a resource group named RG2. The solution must ensure that sa1 can only be accessed from Subnet2.

What should you run?

A: `az storage account network-rule add --resource-group "RG2" --account-name "SA1" --ip-address "10.0.2.0/24" az storage account update --default-action deny --name sa1 --resource-group RG2`

The correct CLI command adds a rule to allow access from the 10.0.2.0/24 subnet to the storage account. The resource group should be for RG2, not RG1. The CLI commands that create network security group (NSG) rules simply allow the entire virtual network to send requests to all storage endpoints.

Q: You have an Azure App Service web app named App1.

Users authenticate to App1 by using Microsoft Entra.

You plan to implement network security controls for App1.

You need to ensure that only authenticated users from your corporate network can sign in to App1. The solution must not require the configuration of virtual network rules.

Which two actions should you perform? Each correct answer presents part of the solution.

A: Configure App Service authentication. & Configure network conditions to Conditional Access.

You can configure the network conditions to Conditional Access, ensuring that the location is determined by the public IP address a client provides to Microsoft Entra or the GPS coordinates provided by the Microsoft Authenticator app. You need to configure App Service authentication, specifically the Action to take when request is not authenticated option. Application security groups are unsupported by web apps and configuring a Front Door IP restriction rule does not affect sa1.

Q: You have an Azure subscription that contains the following resources:

- Storage accounts
- Virtual machines
- Azure Firewall
- Azure Key Vault
- Azure SQL databases

Which three resources support service endpoints? Each correct answer presents a complete solution.

A: Azure Key Vault, Azure SQL databases, & storage accounts

You can configure service endpoints for Azure Storage, Key Vault, and Azure SQL Database. You cannot configure service endpoints for virtual machines and Azure Firewall.

Q: You have an Azure subscription that contains a virtual network named VNet1.

You plan to deploy an Azure App Service web app named Web1.

You need to be able to deploy Web1 to the subnet of VNet1. The solution must minimize costs.

Which pricing plan should you use for Web1?

A: Isolated

Only the Isolated pricing plan (tier) can be deployed to a virtual network subnet. With other pricing plans, inbound traffic is always routed to the public IP address of the web app, while web app outbound traffic can reach the endpoints on a virtual network.

Q: You have an Azure subscription that contains a web app named WebApp1 and a virtual network named VNet1.

VNet1 contains the following subnets:

- Subnet1: Connected to a virtual machine
- Subnet2: Has a `Microsoft.Storage` service endpoint
- Subnet3: Has subnet delegation to the `Microsoft.Sql/managedInstances` service
- Subnet4: Has no additional configurations

You need to integrate WebApp1 with VNet1.

To which subnets can you connect WebApp1?

A: Subnet2 and Subnet4 only

You can integrate a web app only to a dedicated subnet of a virtual network that does not have any connected resources. The subnet can have service endpoints, but subnet delegation should either not be configured or must be configured to the `Microsoft.Web/serverFarms` service, otherwise you will get the following error: Subnet is missing a delegation to `Microsoft.Web/serverFarms`. Please add the delegation and try again. For this scenario, you can integrate WebApp1 with Subnet2 and Subnet4 only.

Q: You have an Azure subscription that contains a virtual network named VNet1.

VNet1 contains the following subnets:

- Subnet1: Has a connected virtual machine
- Subnet2: Has a `Microsoft.Storage` service endpoint
- Subnet3: Has subnet delegation to the `Microsoft.Web/serverFarms` service
- Subnet: Has no additional configurations

You need to deploy an Azure SQL managed instance named managed1 to VNet1.

To which subnets can you connect managed1?

A: Subnet2, Subnet3, and Subnet4 only

You can deploy an SQL managed instance to a dedicated virtual network subnet that does not have any resource connected. The subnet can have a service endpoint or can be delegated for a different service. For this scenario, you can deploy managed1 to Subnet2, Subnet3, and Subnet4 only. You cannot deploy managed1 to Subnet1 because Subnet1 has a connected virtual machine.

Q: You have an Azure App Service web app named App1.

You need to configure network controls for App1. App1 must only allow user access through Azure Front Door.

Which two components should you implement? Each correct answer presents part of the solution.

A: access restrictions based on service tag & header filters

Traffic from Front Door to the app originates from a well-known set of IP ranges defined in the `AzureFrontDoor.Backend` service tag. This includes every Front Door. To ensure traffic only originates from your specific instance, you will need to further filter the incoming requests based on the unique HTTP header that Front Door sends.

Q: You have an Azure subscription that contains a virtual machine named VM1. VM1 runs a web app named App1.

You need to protect App1 by implementing Web Application Firewall (WAF).

What resource should you deploy?

A: Azure Application Gateway

WAF is a tier of Application Gateway. If you want to deploy WAF, you must deploy Application Gateway and select the WAF or WAF V2 tier.

Q: You have an Azure subscription that contains a web app named WebApp1.

You need to recommend a web traffic security and management solution. The solution must meet the following requirements:

- Support SSL off-loading.
- Provide host header routing.
- Provide application load balancing

What should you include in the recommendation?

A: Azure Application Gateway

Application Gateway is a web traffic load balancer that enables you to manage traffic to web apps. Application Gateway works at Layer 7, which enables routing based on path or host headers as well as SSL off-loading. Front Door is implemented globally. Traffic Manager is implemented globally and does not allow path-based routing. Azure Load Balancer does not provide path-based routing.

Q: Your company has an Azure subscription and an Amazon Web Services (AWS) account.

You plan to deploy Kubernetes to AWS.

You need to ensure that you can use Azure Monitor Container insights to monitor container workload performance.

What should you deploy first?

A: Azure Arc-enabled Kubernetes

Azure Arc-enabled Kubernetes is the only configuration that includes Kubernetes and can be deployed to AWS.

Q: You have an Azure subscription that contains an Azure container registry named CR1.

You use Azure CLI to authenticate to the subscription.

You need to authenticate to CR1 by using Azure CLI.

Which command should you run?

A: `az acr login`

The `az acr login` command is needed to authenticate to an Azure container registry from the Azure CLI. Docker login is used to sign in to a Docker repository. `az acr config` is used for configuring Azure Container Registry. `az acr credential` is used for managing login credentials for Azure Container Registry.

Q: You have an Azure Blob storage account named sa1 and an app named App1. A binary file named File1 is stored in sa1.

You need to share File1 with a customer. The solution must limit access to the IP address that the customer used when completing a purchasing flow in App1. Other users must be prevented from accessing File1.

What should you configure?

A: a SAS token that includes the `signedIP` field

Configuring a SAS token that includes the `signedIP` field specifies an IP address or a range of IP addresses from which to accept requests. Configuring network conditions to Conditional Access and a Front Door IP restriction rule affects App1, but not sa1. Configuring a storage account firewall allows access to all the files in sa1, not just File1.

Q: You need to enable encryption at rest by using customer-managed keys (CMKs).

Which two services support CMKs? Each correct answer presents a complete solution.

A: Azure Blob storage & Azure Files

Blob storage and Azure Files both support customer-managed keys. Azure Disk Storage, Azure NetApp Files, and Data Lake Storage do not support customer-managed keys.

Q: You have a storage account that contains multiple containers, blobs, queues, and tables.

You need to create a key to allow an application to access only data from a given table in the storage account.

Which authentication method should you use for the application?

A: service SAS

A SAS service is the only type of authentication that provides control at the table level. User delegation SAS is only available for Blob storage. SAS and shared allow access to the entire storage account.

Q: You need to implement access control for Azure Files. The solution must provide the highest level of security.

What should you use?

A: Microsoft Entra

Microsoft Entra is supported by Azure Files and follows the principle of least privilege. SAS is unsupported by Azure Files. A storage account key is supported by Azure Files, but it does not follow the principle of least privilege.

Q: You have an Azure SQL Database server named Server1 that contains a database named DB1.

You create an auditing policy for DB1.

After a few weeks, you create five more databases in Server1. You then create a new auditing policy for Server1.

You notice that auditing entries for DB1 are duplicated.

You need to ensure that auditing entries for all existing and future databases are not duplicated.

What should you do?

A: Disable auditing for DB1.

Disabling auditing for DB1 will stop duplication. Creating a policy for each of the five new databases or configuring the policy used on Server1 with the same settings as the policy in DB1 will duplicate entries for all databases. Configuring the policy used in DB1 with the same settings as the policy on Server1 will still duplicate entries.

Q: You have an application that runs on-premises and stores data in an Azure SQL database.

You need to ensure that certain columns stored in the database can only be decrypted by the application and cannot be accessed by users managing Azure SQL.

What should you enable for the database?

A: Always Encrypted

Enabling Always Encrypted saves the encrypted data and only the client driver can decrypt it. TDE still allows users managing the database to see data. Dynamic data masking does not encrypt anything, it just masks data and still allows users to unmask it at the database level if they have UNMASK permissions. Symmetric key encryption uses keys stored in a SQL database, not the client application.

Q: You implement dynamic data masking for an Azure Synapse Analytics workspace.

You need to provide only a user named User1 with the ability to see the data.

What should you do?

A: Grant the UNMASK permission to User1.

Granting the UNMASK permission to User1 removes the mask from User1 only. Creating a Conditional Access policy for Azure SQL Database, and then granting access is not enough for User1 to see the data, only to sign in. Using the `ALTER TABLE` statement to edit the masking function affects all users. Using the `ALTER TABLE` statement to drop the masking function removes the mask altogether.

Q: You need to provide public anonymous access to a file in an Azure Storage account. The solution must follow the principle of least privilege.

Which two actions should you perform? Each correct answer presents part of the solution.

A: For the container, set Public access level to **Blob**. & For the storage account, set Blob public access to **Enabled**.

Unless prevented by another setting, setting Public access level to **Blob** allows public access to the blob only. Setting Blob public access to **Enabled** is a prerequisite for setting the access level of container or blob. Setting Blob public access to **Disabled** prevents any public access and setting Public access level to **Container** also allows any current and future blobs in the container, which does not follow the principle of least privilege.

Q: You create an Azure policy by using the following snippet.

```json
"then": {
    "effect": "",
    "details": [{
        "field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules",
        "value": [{
            "action": "Allow",
            "value": "134.5.0.0/21"
        }]
    }]
}
```

You need to ensure that the policy is applied whenever a new storage account is created or updated. There is no managed identity assigned to the policy initiative.

Which effect should you use?

A: Append

`Append` is used to add fields to existing properties. `Modify` is used to add, update, or remove properties, it does not ensure that a field has value. `DeployIfNotExists` is used to deploy resources. `Audit` is used to check for compliance.

Q: You have the following security policy deployed to an Azure subscription.

```json
policyRule: {
  if: {
    allOf: [
      {
        field: "type",
        equals: "Microsoft.Storage/storageAccounts"
      },
      {
        field: "Microsoft.Storage/storageAccounts/allowSharedKeyAccess",
        equals: "true"
      }
    ]
  },
  then: {
    effect: "Deny"
  }
}
```

You successfully deploy a new storage account.

Which statements is true?

A: Usage of Microsoft Entra authentication is enforced.

Enforcing Microsoft Entra authentication prevents using shared keys, and leaves only data plane RBAC as an authentication option. The policy prevents account shared keys for storage accounts. The Storage Account Contributor role is not a data plane RBAC role, but leverages shared keys. SAS tokens can still be created by using a delegated SAS model (Microsoft Entra).

Q: You need to implement an Azure Policy initiative to monitor and enforce compliance for a payment processing service.

Which policy initiative should you use?

A: PCI DSS

The PCI DSS standard covers credit card payment processing. Azure Security Benchmark controls are part of generic Azure Security Benchmark and are not industry specific. The CIS and NIST controls are not industry specific.

Q: You are configuring an Azure Policy in your environment.

You need to ensure that any resources that are missing a tag named CostCenter inherit a value from a resource group.

You create a custom policy that uses the following snippet.

```json
"policyRule": {
    "if": {
        "field": "tags['CostCenter']",
        "exists": "false"
    },
    "then": {
        "effect": "modify",
        "details": {
            "roleDefinitionIds": [
                "/providers/microsoft.authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c"
            ],
            "operations": [{
                "operation": "addOrReplace ",
                "field": "tags['CostCenter']",
                "value": "[resourcegroup().tags['CostCenter']]"
            }]
        }
    }
}
```

Which policy mode should you use?

A: indexed

`indexed` mode ensures that the policy skips resource groups. `all` includes resource groups, which cannot be nested. `Append` and `DeployIfNotExists` are policy effects.

Q: Your company has a multi-cloud online environment.

You plan to use Microsoft Defender for Cloud to protect all supported online environments.

Which three environments support Defender for Cloud? Each correct answer presents a complete solution.

A: Amazon Web Services (AWS), Azure DevOps, & GitHub

Defender for Cloud protects workloads in Azure, AWS, GitHub, and Azure DevOps. Oracle Cloud and Alibaba Cloud are unsupported by Defender for Cloud.

Q: You have an Azure subscription that contains a user named Admin1.

You need to ensure that Admin1 can create and assign custom security initiatives in Microsoft Defender for Cloud. The solution must follow the principle of least privilege.

Which role should you assign to Admin1?

A: Owner (Subscription)

The Subscription Owner role is the only role that has permissions to create and assign custom security initiatives in Defender for Cloud.

Q: You have an Azure subscription.

You need to recommend a solution that uses crawling technology of Microsoft to discover and actively scan assets within an online infrastructure. The solution must also discover new connections over time.

What should you include in the recommendation?

A: Microsoft Defender External Attack Surface Management (EASM)

Defender EASM applies the crawling technology of Microsoft to discover assets that are related to your known online infrastructure and actively scans these assets to discover new connections over time. Attack Surface Insights are generated by applying vulnerability and infrastructure data to showcase the key areas of concern for your organization.

Q: You are implementing a Microsoft Defender for SQL vulnerability assessments.

Where are the scan results stored?

A: an Azure Storage account

The scan results must be stored in an Azure Storage account. The results can be sent to the Azure Monitor workspace or Microsoft Sentinel from the initial location. The results are stored outside of the database.

Q: You are implementing Microsoft Defender for SQL vulnerability assessments.

In which two locations can users view the results? Each correct answer presents a complete solution.

A: an Azure Blob storage account & Microsoft Defender for Cloud

Defender for Cloud is the default and mandatory location to view the results, while a Blob storage account is a mandatory destination and a prerequisite for enabling the scan. The Teams option is unavailable out of the box. A scan completion event is not sent to Event Grid.

Q: You have an Azure subscription and the following SQL deployments:

- An Azure SQL database named DB1
- An Azure SQL Server named sqlserver1
- An instance of SQL Server on Azure Virtual Machines named VM1 that has Microsoft SQL Server 2022 installed
- An on-premises server named Server1 that has SQL Server 2019 installed

Which deployments can be protected by using Microsoft Defender for Cloud?

A: DB1, sqlserver1, VM1, and Server1

Defender for Cloud includes Microsoft Defender for SQL. Defender for SQL can protect Azure SQL Database, Azure SQL Server, SQL Server on Azure Virtual Machines, and SQL servers installed on on-premises servers.

Q: You configure a Linux virtual machine to send Syslog data to Microsoft Sentinel.

You notice that events for the virtual machine are duplicated in Microsoft Sentinel.

You need to ensure that the events are not duplicated.

Which two actions should you perform? Each correct answer presents part of the solution.

A: Disable the synchronization of the Log Analytics agent with the Syslog configuration in Microsoft Sentinel. & Remove the entry used to send CEF messages from the Syslog configuration file for the virtual machine.

You must disable CEF messages on the virtual machine and prevent the setting to send CEF messages from being read. Stopping the Syslog daemon on the virtual machine will stop the virtual machine from sending both Syslog and CEF messages. Enabling the Syslog daemon to listen and disabling the Syslog daemon from listening to network messages does not handle the duplication of events.

Q: You are configuring retention for Azure activity logs in Azure Monitor logs. The retention period for the Azure Monitor logs is set to 30 days.

You need to meet the following compliance requirements:

- Store the Azure activity logs for 90 days.
- Encrypt the logs by using your own encryption keys.
- Use the most cost-efficient storage solution for the logs.

What should you do?

A: Configure diagnostic settings and send the logs to Azure Storage. & Configure diagnostic settings and send the logs to Azure Storage.

Configuring diagnostic settings and sending the logs to Azure Storage meets both the retention time and encryption requirements. Activity log data type is kept for 90 days by default, but the logs are stored by using Microsoft-managed keys. Configuring a workspace retention policy is not the most cost-efficient solution for this. Event Hubs is a real-time event stream engine and is not designed to be used instead of a database or as a permanent store for indefinitely held event streams.

Q: You are designing an Azure solution that stores encrypted data in Azure Storage.

You need to ensure that the keys used to encrypt the data cannot be permanently deleted until 60 days after they are deleted. The solution must minimize costs.

What should you do?

A: Store keys in a software-protected key vault that has soft delete and purge protection enabled.

Purge protection prevents keys from being permanently deleted for a certain number of days, and software-protected key vaults are less expensive than HSM-protected key vaults. Without purge protection, the keys are not protected from being permanently deleted for 60 days. An HSM-protected key vault is more expensive than a software-backed key vault.

Q: You need to grant an application access to read connection strings stored in Azure Key Vault. The solution must follow the principle of least privilege.

Which role assignment should you use?

A: Key Vault Secrets User

Key Vault Secrets User allows read access to secret content. Key Vault Crypto Officer allows the user to perform actions on encryption keys, not secrets. Key Vault Reader allows the user to read the metadata of key vaults and its certificates, keys, and secrets, but not to read sensitive values, such as secret contents or key material. Key Vault Secrets Officer does not follow the principle of least privilege.

Q: You have an Azure tenant that contains a user named User1 and an Azure key vault named Vault1. Vault1 is configured to use Azure role-based access control (RBAC).

You need to ensure that User1 can perform actions on keys in Vault1 but cannot manage permissions. The solution must follow the principle of least privilege.

Which role should you assign to User1?

A: Key Vault Crypto Officer

Correct: Key Vault Crypto Officer –– This role meets the requirements.

Incorrect: Key Vault Secrets Officer –– This role is for secrets, not keys.

Incorrect: Key Vault Reader –– This role is only for read access, not performing actions.

Incorrect: Key Vault Crypto User –– This role can manage permissions.

Incorrect: Key Vault Secrets User –– This role is for secrets, not keys.