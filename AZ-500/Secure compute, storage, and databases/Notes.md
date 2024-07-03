# Plan and implement advanced security for compute

## Plan and implement remote access to public endpoints, including Azure Bastion and just-in-time (JIT) virtual machine (VM) access
**Azure Bastion Overview:**
- Secure RDP/SSH access via browser/Azure portal.
- No public IP, agent, or special software needed.
- Fully managed PaaS service.

**Key Benefits:**
- **RDP/SSH via Azure Portal**: One-click access.
- **Secure Sessions**: TLS over port 443, supports TLS 1.2+.
- **Private IP Usage**: No public IP required for VMs.
- **Simplified NSG Management**: Only private IP access needed.
- **Managed Service**: Azure Bastion is maintained by Azure.
- **Port Scanning Protection**: No internet exposure for VMs.
- **Centralized Hardening**: Only Bastion needs hardening.
- **Zero-Day Protection**: Automatically updated by Azure.

**SKUs:**
- **Basic**: Same VNet, peered VNet, concurrent connections, AKV keys, SSH, RDP, Kerberos.
- **Standard**: Adds custom ports, Azure CLI, host scaling, file transfer, shareable link, IP address connection, disable copy/paste, VM audio output.

**Architecture:**
- Deployed in VNet with `AzureBastionSubnet` (/26 prefix).
- RDP/SSH via browser; no public IP on VMs.
- Supports VNet peering.

**Host Scaling:**
- Manual scaling up to 50 instances.
- Only available in Standard SKU.

**JIT VM Access:**
- Locks down VMs, allows access on-demand.
- Specify ports and access duration.
- Access requests approved by authorized users.

**Implementation Steps:**
1. **Create Bastion Host**: In VNet, create `AzureBastionSubnet` (/26) and deploy Bastion.
2. **Connect to VM**: Select VM in Azure portal, click `Connect`, choose `Bastion`, and enter credentials.
3. **Manage Settings**: Configure scaling, policies in Azure portal.

### JIT VM Access
- **Purpose**: Reduce attack surface, controlled VM access.
- **Configuration**: Enable JIT, specify ports and duration, authorize access requests.
## Configure network isolation for Azure Kubernetes Service (AKS)
### What is Azure Kubernetes Service?
Azure Kubernetes Service (AKS) is a managed Kubernetes service that you can use to deploy and manage containerized applications. You don't need container orchestration expertise to use AKS. AKS reduces the complexity and operational overhead of managing Kubernetes by offloading much of that responsibility to Azure. AKS is an ideal platform for deploying and managing containerized applications that require high availability, scalability, and portability, and for deploying applications to multiple regions, using open-source tools, and integrating with existing DevOps tools.
### Overview of Azure Kubernetes Service
AKS reduces the complexity and operational overhead of managing Kubernetes by shifting that responsibility to Azure. When you create an AKS cluster, Azure automatically creates and configures a control plane for you at no cost. The Azure platform manages the AKS control plane, which is responsible for the Kubernetes objects and worker nodes that you deploy to run your applications. Azure takes care of critical operations like health monitoring and maintenance, and you only pay for the AKS nodes that run your applications.
### AKS Network Isolation

**Overview:**
- Isolate teams and workloads in AKS for multi-tenancy.
- Understand AKS isolation features to optimize Kubernetes investment.

**Key Concepts:**

1. **Design Clusters for Multi-Tenancy:**
   - **Kubernetes Namespace**: Logical isolation boundary.
   - **Resource Isolation Areas**:
     - **Scheduling**: Resource quotas, pod disruption budgets, taints/tolerations, node selectors, affinity/anti-affinity.
     - **Networking**: Network policies control traffic flow.
     - **Authentication/Authorization**: RBAC, Microsoft Entra, pod identities, Azure Key Vault secrets.
     - **Containers**: Azure Policy add-on, pod security admission, vulnerability scanning, App Armor/Seccomp.

2. **Logical Isolation:**
   - Use a single AKS cluster with multiple namespaces for different teams/workloads.
   - **Advantages**: Higher pod density, cost-effective (autoscaler adjusts node count).
   - **Diagram**: Shows logically isolated clusters.
   - **Considerations**: Not entirely safe for hostile multitenant usage; use RBAC and trust hypervisors for true security.

3. **Physical Isolation:**
   - Assign separate AKS clusters to different teams/workloads.
   - **Advantages**: True isolation for hostile environments.
   - **Disadvantages**: Increased management/financial overhead, low pod density, excess idle resources.
   - **Diagram**: Shows physically isolated clusters.
   - **Best Practice**: Use physical isolation minimally; prefer logical isolation.

**Best Practices:**
- Use logical isolation for higher efficiency and lower costs.
- Physical isolation only for untrusted, hostile multitenant workloads.
- Implement RBAC and additional security measures for shared environments.
## Secure and monitor AKS
**Overview:**
- Microsoft Defender for Containers enhances the security of containerized assets (Kubernetes clusters, nodes, workloads, container registries, container images, etc.) across multicloud and on-premises environments.

**Core Domains:**

1. **Security Posture Management:**
   - **Continuous Monitoring**: Monitors cloud and Kubernetes APIs and workloads.
   - **Inventory Capabilities**: Discovers resources and detects misconfigurations.
   - **Guidelines & Risk Assessment**: Provides mitigation guidelines and risk assessments.
   - **Risk Hunting**: Enables enhanced risk hunting through security explorer.

2. **Vulnerability Assessment:**
   - **Agentless Assessment**: Covers Azure, AWS, GCP with daily rescans.
   - **Remediation Guidelines**: Provides detailed remediation steps and insights.
   - **Coverage**: Includes OS and language packages.
   - **Contextual Risk**: Adds vulnerability info to security graph for attack path calculations.

3. **Run-Time Threat Protection:**
   - **Threat Detection Suite**: Monitors clusters, nodes, and workloads.
   - **Threat Intelligence**: Maps threats to the MITRE ATT&CK framework.
   - **Automated Response**: Integrates with SIEM/XDR for automated response.

4. **Deployment & Monitoring:**
   - **Agent Monitoring**: Ensures agents are present and functioning.
   - **Standard Tools**: Supports Kubernetes monitoring tools.
   - **Unmonitored Resources**: Manages and alerts on unmonitored resources.

**Capabilities:**

1. **Agentless Capabilities:**
   - **Discovery**: API-based discovery of clusters and configurations.
   - **Vulnerability Assessment**: Assesses container images for vulnerabilities.
   - **Inventory Management**: Monitors resources, pods, services, images.
   - **Risk-Hunting**: Security admins can hunt for posture issues.
   - **Control Plane Hardening**: Assesses and recommends fixes for cluster configurations.

2. **Agent-Based Capabilities:**
   - **Data Plane Hardening**: Uses Azure Policy for Kubernetes to enforce best practices.
   - **Best Practices Enforcement**: Blocks non-compliant requests, e.g., privileged containers.

3. **Vulnerability Assessment:**
   - **Container Image Scans**: Scans images in ACR and AWS ECR.
   - **Reports**: Provides detailed vulnerability reports and remediation guidance.
   - **Solutions**: Powered by Microsoft Defender Vulnerability Management and Qualys.

4. **Run-Time Protection:**
   - **Real-Time Alerts**: Monitors and alerts on suspicious activities.
   - **Cluster Level Protection**: Uses Defender agent and Kubernetes audit logs.
   - **Examples of Monitored Events**:
     - Exposed dashboards.
     - Creation of high privileged roles.
     - Creation of sensitive mounts.
   - **Viewing Alerts**: Security alerts can be viewed in Defender for Cloud.

**Security Alerts:**
- **Viewing Alerts**: Alerts are visible under Security alerts in Defender for Cloud.
- **Alert Types**: Alerts for runtime workloads have the prefix K8S.NODE_.
- **Threat Detection**: Includes over 60 Kubernetes-aware analytics and anomaly detections.

**Additional Notes:**
- **MITRE ATT&CK Matrix**: Defender for Containers uses the MITRE ATT&CK matrix for threat monitoring.
- **Host-Level Detection**: Includes host-level threat detection for comprehensive security.
## Configure authentication for AKS
**Overview:**
- Authentication in Azure Kubernetes Service (AKS) ensures controlled access to resources and services, preventing unauthorized access and facilitating credential management.

**Authentication Methods:**

1. **Microsoft Entra ID Integration:**
   - **Best Practice**: Deploy AKS clusters with Microsoft Entra integration for centralized identity management.
   - **Integration**: Create roles and bindings to define access permissions, linking with Microsoft Entra ID.
   - **Benefits**: Automatic updates for user/group status changes, minimal permissions through RBAC.

2. **Kubernetes Role-Based Access Control (RBAC):**
   - **Usage**: Define granular access control to cluster resources.
   - **Roles & Bindings**: Assign least amount of permissions required to users/groups.
   - **Integration**: Integrate with Microsoft Entra ID for automated updates.

3. **Azure Role-Based Access Control (RBAC):**
   - **Usage**: Define minimum required permissions to AKS resources across subscriptions.
   - **Access Levels**:
     - Access to AKS resource on Azure subscription.
     - Access to Kubernetes API.
   - **Integration**: Integrate with Kubernetes RBAC or Azure RBAC for authorization.

4. **Pod-Managed Identities:**
   - **Best Practice**: Avoid fixed credentials, use pod-managed identities for secure access.
   - **Functionality**: Automatically requests access through Microsoft Entra ID.
   - **Modes**:
     - Standard Mode: Deploy Managed Identity Controller (MIC) and Node Managed Identity (NMI).
     - Managed Mode: Identity assignment pre-managed, no MIC required.

**Pod-Managed Identity Operation:**

- **Standard Mode**:
  - **Components**: MIC and NMI deployed to AKS cluster.
  - **MIC**: Assigns managed identity to node pool during pod creation.
  - **NMI**: Intercepts token requests and validates pod access.

- **Managed Mode**:
  - **Advantages**: Pre-assigned identity to node pool, no MIC required.
  - **Operations**:
    - Node Management Identity (NMI) server handles pod requests.
    - Azure Resource Provider queries Kubernetes API for identity mapping.
    - NMI server requests access token from Microsoft Entra ID for pod.

**Example Scenario:**
- **Pod Authentication with Managed Identity**:
  - **Setup**: Cluster operator maps identities using service account.
  - **Deployment**: NMI server deployed to relay pod requests to Microsoft Entra ID.
  - **Usage**: Pod with managed identity requests access token through NMI server.
  - **Access**: Token returned to pod, used to access Azure resources like Azure SQL Database.
## Configure security monitoring for Azure Container Instances (ACIs)
**Overview:**
- Azure Monitor provides insights into resource usage and network activity for Azure Container Instances (ACIs), aiding in optimizing resource settings and monitoring container performance.

**Metrics Available:**
- **CPU Usage**: Measured in millicores, representing usage of CPU cores.
- **Memory Usage**: Measured in bytes.
- **Network Activity**:
  - Bytes received per second.
  - Bytes transmitted per second.

**Accessing Metrics:**

1. **Azure Portal**:
   - Metrics available in the Overview page for container groups.
   - Pre-created charts for each metric.
   - Ability to view individual container metrics by applying dimension splitting.

2. **Azure CLI**:
   - Metrics retrieval using Azure CLI.
   - Use commands to get the ID of the container group.
   - Replace placeholders with actual resource group and container group names.

**Preview Limitations:**
- Azure Monitor metrics currently only available for Linux containers.

**Example Usage (Azure CLI):**
- To get container group ID:
  ```
  az container show --resource-group <resource-group> --name <container-group> --query id
  ```
## Configure security monitoring for Azure Container Apps (ACAs)
**Private Registry Usage:**
- Store container images in private registries like Azure Container Registry to enhance security.
- Publicly available images may contain vulnerabilities; private registries offer better control and security.

**Image Monitoring and Scanning:**
- Utilize scanning solutions (e.g., Microsoft Defender for Cloud, Twistlock, Aqua Security) to identify and mitigate image vulnerabilities.
- Azure Container Registry integrates with Microsoft Defender for Cloud to scan Linux images automatically.

**Credential Protection:**
- Secure credentials used for logins or API access.
- Inventory and manage credential secrets using secrets-management tools.
- Azure Key Vault safeguards encryption keys and secrets for containerized applications.

**Container Ecosystem Security:**
- Implement vulnerability management throughout the container lifecycle.
- Scan images for vulnerabilities before pushing to registries.
- Map image vulnerabilities to running containers for mitigation.

**Approved Images and Registries:**
- Permit only approved container images and registries to reduce exposure to risk.
- Utilize image signing or fingerprinting for integrity verification.

**Continuous Integration Pipeline:**
- Use CI pipelines with integrated security scanning to build and push secure images.

**Least Privileges in Runtime:**
- Operate containers with minimal privileges to reduce risk.
- Remove unused processes or privileges from container runtimes.

**Network Segmentation and Monitoring:**
- Enforce network segmentation to protect containers.
- Monitor container activity and user access for early detection of suspicious behavior.

**Azure Monitoring Solutions:**
- Azure Monitor for containers provides performance visibility and monitoring.
- Azure Container Monitoring solution offers centralized management and monitoring of Docker and Windows container hosts.
- Utilize Azure Monitor for core monitoring and alerting.

**Audit Logging:**
- Log all administrative user access for auditing purposes.
- Integration with Azure services like Microsoft Defender for Cloud and Azure Container Monitoring provides additional security insights and recommendations.
## Manage access to Azure Container Registry (ACR)
**Azure RBAC Built-in Roles:**
- Azure Container Registry (ACR) provides built-in Azure roles for managing permissions.
- Roles include Owner, Contributor, Reader, AcrPush, AcrPull, AcrDelete, and AcrImageSigner.

**Assign Roles:**
- Use Azure portal, Azure CLI, Azure PowerShell, or other Azure tools to assign roles.
- When creating a service principal, configure its access and permissions to ACR.

**Differentiate Users and Services:**
- Apply limited permissions based on the task requirements.
- Assign roles such as AcrPush for CI/CD solutions and AcrPull for container host nodes.

**CI/CD Solutions:**
- Assign AcrPush role for automated docker build commands.
- Prevents broader operations and Azure Resource Manager access.

**Container Host Nodes:**
- Assign AcrPull role to nodes running containers.
- Reader capabilities are not required.

**Visual Studio Code Docker Extension:**
- Provide Reader or Contributor role access for Visual Studio Code Docker extension users.
- Enables docker pull, docker push, and other capabilities.

**Access Resource Manager:**
- Required for Azure portal and Azure CLI registry management.
- Enables commands like az acr list to get a list of registries.

**Permissions Overview:**
- Create/delete registry: Ability to manage registry creation and deletion.
- Push image: Permission to push images to the registry.
- Pull image: Ability to pull images from the registry.
- Delete image data: Permission to delete images from the registry.
- Change policies: Ability to configure registry policies.
- Sign images: Permission to sign images, often combined with push image for trusted image management.

**Custom Roles:**
- Create custom roles with fine-grained permissions for ACR.
- Assign custom roles to users, service principals, or other identities as needed.
## Configure disk encryption, including Azure Disk Encryption (ADE), encryption at host, and confidential disk encryption
**Encryption at Rest Overview:**
- Encryption at Rest secures data stored in Azure without requiring custom key management solutions.
- Symmetric encryption is used for encrypting and decrypting data quickly.

**Purpose of Encryption at Rest:**
- Protects data stored on disk from unauthorized access.
- Helps meet compliance requirements like HIPAA and PCI.
- Provides defense-in-depth protection in case other security measures fail.

**Azure Encryption at Rest Components:**
1. **Azure Key Vault:** Centralized key storage solution for managing encryption keys securely.
2. **Microsoft Entra ID:** Permissions to access keys stored in Azure Key Vault.
3. **Envelope Encryption:** Uses a Key Hierarchy with Data Encryption Keys (DEK) and Key Encryption Keys (KEK) for better security and performance.

**Encryption at Rest in Microsoft Cloud Services:**
- **SaaS Customers:** Encryption options available within each service (e.g., Microsoft 365).
- **PaaS Customers:** Data stored in services like Blob Storage, with encryption options based on service capabilities.
- **IaaS Customers:** Encryption at rest enabled for virtual machines and disks using Azure Disk Encryption.

**Azure Resource Providers Encryption Support:**
- **Azure Disk Encryption:** Achieves encryption at rest for IaaS VMs and disks.
- **Azure Storage:** Supports server-side encryption by default, customer-managed keys, and client-side encryption.
- **Azure SQL Database:** Supports Transparent Data Encryption (TDE) for server-side encryption and Always Encrypted for client-side encryption.

**Conclusion:**
- Azure is committed to providing Encryption at Rest options across its services.
- Services support service-managed keys, customer-managed keys, or client-side encryption.
- New options for Encryption at Rest are continuously being developed and enhanced.
## Recommend security configurations for Azure API Management
**Network Security:**
- **NS-1: Establish network segmentation boundaries**
  - *Feature:* Virtual Network Integration
  - *Configuration Guidance:* Deploy Azure API Management inside an Azure Virtual Network (VNET) for backend service access. Choose between External (accessible from the internet) or Internal (accessible only within Vnet) configurations.
  
- **NS-2: Secure cloud services with network controls**
  - *Feature:* Azure Private Link, Disable Public Network Access
  - *Configuration Guidance:* Utilize Azure Private Link for private access points if unable to deploy into a virtual network. Disable public network access either through IP ACL filtering or toggle switch.

**Identity Management:**
- **IM-1: Use centralized identity and authentication system**
  - *Feature:* Azure AD Authentication Required for Data Plane Access
  - *Configuration Guidance:* Utilize Azure AD authentication for data plane access. Configure Developer Portal and APIs to use OAuth 2.0 with Azure AD.

- **IM-3: Manage application identities securely and automatically**
  - *Feature:* Managed Identities, Service Principals
  - *Configuration Guidance:* Use Managed Service Identity for authentication instead of service principals. Managed identity credentials are managed by Azure platform.

**Privileged Access:**
- **PA-1: Separate and limit highly privileged/administrative users**
  - *Feature:* Local Admin Accounts
  - *Configuration Guidance:* Disable local admin accounts unless required for emergency use. Prefer Azure AD authentication.

- **PA-7: Follow just enough administration (least privilege) principle**
  - *Feature:* Azure RBAC for Data Plane
  - *Configuration Guidance:* Use Azure RBAC for fine-grained access control to API Management services and entities.

**Data Protection:**
- **DP-3: Encrypt sensitive data in transit**
  - *Feature:* Data in Transit Encryption
  - *Configuration Guidance:* Default deployment enables TLS for data in transit.

- **DP-6: Use a secure key management process**
  - *Feature:* Key Management in Azure Key Vault
  - *Configuration Guidance:* Integrate API Management with Azure Key Vault for storing and accessing keys securely.

**Asset Management:**
- **AM-2: Use only approved services**
  - *Feature:* Azure Policy Support
  - *Configuration Guidance:* Use Azure Policy to monitor and enforce secure configurations across API Management resources.

**Logging and Threat Detection:**
- **LT-1: Enable threat detection capabilities**
  - *Feature:* Microsoft Defender for Service / Product Offering
  - *Configuration Guidance:* Not supported for API Management.

**Backup and Recovery:**
- **BR-1: Ensure regular automated backups**
  - *Feature:* Azure Backup, Service Native Backup Capability
  - *Configuration Guidance:* Utilize Azure API Management's backup and restore capabilities, storing backups in customer-owned Azure Storage accounts.
# Plan and implement security for storage

## SAS
A service shared access signature (SAS) delegates access to a resource in just one of the storage services: Azure Blob Storage, Azure Queue Storage, Azure Table Storage, or Azure Files. The URI for a service-level SAS consists of the URI to the resource for which the SAS will delegate access, followed by the SAS token.

The SAS token is the query string that includes all the information that's required to authorize a request. The token specifies the resource that a client may access, the permissions granted, and the time period during which the signature is valid.

A SAS can also specify the supported IP address or address range from which requests can originate, the supported protocol with which a request can be made, or an optional access policy identifier that's associated with the request.

Finally, every SAS token includes a signature.
## Azure Storage

- **Platform Overview:**
  - Highly available, scalable, secure storage solution by Microsoft.
  - Accessible globally via REST API, HTTPS, client libraries, Azure PowerShell, Azure CLI, Azure portal, and Azure Storage Explorer.

- **Benefits:**
  - Durable, highly available, secure, scalable, managed, accessible.
  
- **Data Services:**
  - Azure Blobs: Object store for text/binary data and Data Lake Storage Gen2.
  - Azure Files: Managed file shares.
  - Azure Elastic SAN: Simplified SAN deployment.
  - Azure Queues: Messaging store.
  - Azure Tables: NoSQL schemaless data store.
  - Azure managed Disks: Block-level storage for VMs.
  - Azure Container Storage (preview): Volume management for containers.

- **Specialized Storage:**
  - Azure NetApp Files: Enterprise files storage, NFS/SMB support.
  
- **Sample Scenarios:**
  - Azure Files: Lift & shift apps, file sharing.
  - Azure NetApp Files: High-performance, difficult-to-migrate workloads.
  - Azure Blobs: Streaming, data access anywhere.
  - Azure Elastic SAN: Large-scale storage interoperability.
  - Azure Disks: Disk storage for VMs.
  - Azure Container Storage: Persistent volumes for Kubernetes.
  - Azure Queues: Asynchronous messaging between components.
  - Azure Tables: Flexible schemaless data storage.

### Azure Blob Storage

- **Overview:**
  - Object storage solution for the cloud, optimized for unstructured data.
  - Ideal for serving, storing, streaming data, backup, analysis.

- **Access:**
  - Via HTTP/HTTPS, URLs, REST API, client libraries, SSH, NFS 3.0.

### Azure Files

- **Overview:**
  - Highly available network file shares.
  - Accessed via SMB, NFS, REST API, SAS tokens.

- **Usage Scenarios:**
  - Application migration, config files, resource logs, utilities.

### Azure Elastic SAN

- **Overview:**
  - Simplified SAN deployment, scalability, management.
  - Supports large-scale Input/Output-intensive workloads.

### Azure Container Storage (preview)

- **Overview:**
  - Volume management for containers integrated with Kubernetes.
  - Rapid scale out, improved performance, native orchestration.

## Configure access control for storage accounts
- **Overview:**
  - Secure resource requests for Blob, File, Queue, or Table services.
  - Authorization ensures controlled access to storage account resources.

- **Authorization Options:**
  - **Azure Blobs:**
    - Shared Key: Supported
    - Shared Access Signature (SAS): Supported
    - Microsoft Entra ID: Supported
    - On-premises AD DS: Not supported
    - Anonymous Public Read Access: Supported
  - **Azure Files (SMB):**
    - Shared Key: Supported
    - SAS: Not supported
    - Microsoft Entra ID: Supported (with Entra Domain Services/Kerberos)
    - On-premises AD DS: Supported (credentials synced to Entra ID)
    - Anonymous Public Read Access: Not supported
  - **Azure Files (REST):**
    - Shared Key: Supported
    - SAS: Supported
    - Microsoft Entra ID: Supported
    - On-premises AD DS: Not supported
    - Anonymous Public Read Access: Not supported
  - **Azure Queues:**
    - Shared Key: Supported
    - SAS: Supported
    - Microsoft Entra ID: Supported
    - On-premises AD DS: Not supported
    - Anonymous Public Read Access: Not supported
  - **Azure Tables:**
    - Shared Key: Supported
    - SAS: Supported
    - Microsoft Entra ID: Supported
    - On-premises AD DS: Not supported
    - Anonymous Public Read Access: Not supported

- **Authorization Methods:**
  - **Microsoft Entra ID:**
    - Cloud-based identity and access management service.
    - Available for Blob, File, Queue, Table services.
    - Use RBAC for fine-grained access control.
  - **Entra Domain Services:**
    - Supports identity-based authorization over SMB for Azure Files.
    - Use RBAC for share-level and NTFS DACLs for file-level permissions.
  - **Active Directory (AD):**
    - Supports identity-based authorization over SMB for Azure Files.
    - Use AD credentials from domain-joined machines.
    - Use RBAC and NTFS DACLs for permissions.
  - **Shared Key:**
    - Uses account access keys and parameters for encrypted signature.
  - **Shared Access Signatures (SAS):**
    - Delegates access to specific resources with specified permissions/time.
  - **Anonymous Access:**
    - Allows optional public access to blob resources.

- **Recommendations:**
  - Prefer Microsoft Entra ID for superior security and ease of use.
  - Avoid storing account access keys with code (use Entra ID instead).
  - Use Entra ID over SAS for fine-grained access without managing tokens.
## Manage life cycle for storage account access keys
- **Overview:**
  - Azure generates two 512-bit storage account access keys upon creation.
  - Keys authorize data access via Shared Key authorization or SAS tokens.

- **Key Management Recommendations:**
  - **Azure Key Vault:**
    - Recommended for managing and rotating keys securely.
    - Allows key rotation without application interruptions.
  - **Manual Rotation:**
    - Rotate keys periodically.
    - Protect keys by avoiding hard-coding or saving them in plain text.
    - Use SAS tokens for limited access scenarios when Entra ID isn’t available.
    - Regularly rotate keys to mitigate potential compromises.

- **Protecting Access Keys:**
  - Keys grant full access to storage account configuration and data.
  - Limit and monitor access to shared keys.
  - Use Microsoft Entra ID for blob, queue, and table data access.
  - For Azure Files (SMB), use on-premises AD DS or Microsoft Entra Kerberos.
  - Disallow Shared Key authorization to prevent unauthorized access.

- **Viewing Access Keys:**
  - Access via Azure portal, PowerShell, or Azure CLI.
  - In Azure portal:
    - Navigate to the storage account.
    - Select **Access keys** under **Security + networking**.
    - Click **Show keys** to view and copy keys or connection strings.

- **Using Azure Key Vault:**
  - Securely stores and manages access keys.
  - Applications access keys from Key Vault to avoid embedding in code.

- **Manual Key Rotation:**
  - Two keys ensure continuous access during rotation.
  - Steps for rotation:
    - Update connection strings to use the secondary key.
    - Regenerate the primary key in the Azure portal.
    - Update connection strings to use the new primary key.
    - Regenerate the secondary key.

- **Key Rotation Policy:**
  - Set reminders for key rotation.
  - Use Azure Policy to monitor key rotation compliance.
  - Steps to set a rotation reminder in the Azure portal:
    - Navigate to **Access keys** under the storage account.
    - Click **Set rotation reminder** and enable reminders.

- **Monitoring Compliance:**
  - Use built-in Azure policies to ensure keys are rotated regularly.
  - Assign the **Storage account keys should not be expired** policy.
  - Monitor compliance via the Azure Policy dashboard.
  - Rotate non-compliant keys to ensure adherence to the policy.

- **Key Rotation Procedure:**
  - For key rotation permissions, user must be a Service Administrator or assigned to an Azure role with `Microsoft.Storage/storageAccounts/regeneratekey/action`.
  - Built-in roles with this action include Owner, Contributor, and Storage Account Key Operator Service Roles.
## Select and configure an appropriate method for access to Azure Files
#### Overview:
- **Azure Portal Access:** Requests to Azure Files via the Azure portal can be authorized using Microsoft Entra account or storage account access key.
- **Authorization Switching:** The portal allows switching between the two methods if permissions are appropriate.

#### Permissions for Access:
- **Microsoft Entra Account:**
  - Requires assignment to roles that grant file data access.
  - **Azure Resource Manager Reader Role:** Needed at a minimum to view storage account resources but not modify them.
  - **Built-in Roles:**
    - **Storage File Data Privileged Reader**
    - **Storage File Data Privileged Contributor** (can read, write, delete, and modify ACLs/NTFS permissions, though not supported via the portal).
  - **Custom Roles:** Can combine permissions similar to built-in roles.

- **Storage Account Access Key:**
  - Requires an Azure role with `Microsoft.Storage/storageAccounts/listkeys/action`.
  - **Built-in Roles with this Action:**
    - Reader and Data Access
    - Storage Account Contributor
    - Azure Resource Manager Contributor
    - Azure Resource Manager Owner
  - **Key-Based Access Restriction:** If a storage account has a ReadOnly lock, `List Keys` operation isn't allowed, necessitating Microsoft Entra credentials for access.

#### Changing Authentication Method for File Shares:
- **Default Authorization:**
  - Portal uses current authorization method for file shares.
  - Can be switched individually.
  
- **Steps to Change Authorization Method:**
  1. Navigate to storage account in Azure portal.
  2. Select Data storage > File shares.
  3. Select a file share and click Browse.
  4. **Current Authentication Method Displayed:** Indicates Access Key or Microsoft Entra account.
  5. **Switching Methods:**
     - **To Microsoft Entra Account:** Select "Switch to Microsoft Entra user account" link.
     - **To Access Key:** Select "Switch to access key" link.
  
#### Microsoft Entra Account Authentication:
- Requires two additional RBAC permissions:
  - `Microsoft.Storage/storageAccounts/fileServices/readFileBackupSemantics/action`
  - `Microsoft.Storage/storageAccounts/fileServices/writeFileBackupSemantics/action`

#### Default to Microsoft Entra Authorization:
- **New Storage Account:**
  1. Follow instructions to create a storage account.
  2. On the Advanced tab, under Security, check the box for "Default to Microsoft Entra authorization in the Azure portal."
  3. Select Review + create.
  
- **Existing Storage Account:**
  1. Navigate to storage account overview in Azure portal.
  2. Under Settings, select Configuration.
  3. Set "Default to Microsoft Entra authorization in the Azure portal" to Enabled.

#### Summary:
- **Access Methods:**
  - Microsoft Entra account for enhanced security and easier management.
  - Storage account access key for traditional access, limited by higher permission requirements and potential security risks.
- **Flexibility:**
  - Azure portal provides flexibility to switch authorization methods for individual file shares or set defaults for broader access management.
- **Security Best Practices:**
  - Default to Microsoft Entra authorization where possible for improved security.
  - Regularly review and manage permissions to ensure least privilege access.
## Select and configure an appropriate method for access to Azure Blob Storage
#### Overview:
Azure Storage supports using Microsoft Entra ID (formerly Azure AD) to authorize requests to blob data. This allows using Azure role-based access control (RBAC) to grant permissions to users, groups, or application service principals. Microsoft Entra ID authorization is available for all general-purpose and Blob storage accounts created using the Azure Resource Manager model.

#### Microsoft Entra ID for Blob Access:
- **Benefits:** Provides superior security over Shared Key authorization, and is recommended for use with managed identities.
- **Managed Identities:** Used to authorize requests within Azure environments (e.g., Azure VMs, Azure Functions). Managed identities simplify authentication and improve security.
- **External Resources:** On-premises applications can use managed identities through Azure Arc to connect to Azure services.
- **User Delegation SAS:** Secured with Microsoft Entra credentials, recommended over account key-based SAS.

#### Authorization Process:
1. **Authentication:**
   - The security principal's identity is authenticated, returning an OAuth 2.0 token.
   - Applications can request OAuth 2.0 access tokens at runtime using managed identities for Azure resources.
2. **Authorization:**
   - The token is passed to the Blob service to authorize access to the specified resource.

#### Access Methods:
- **Azure Portal, PowerShell, Azure CLI:** Can access blob data using Microsoft Entra credentials. Requires appropriate Azure RBAC roles.

#### Azure Identity Client Library:
- **Purpose:** Simplifies acquiring OAuth 2.0 tokens for Microsoft Entra ID authorization.
- **Usage:** Integrates with Azure SDKs for various languages (e.g., .NET, Java, Python).
- **DefaultAzureCredential Class:** Automatically attempts different credential types to acquire a token, suitable for both development and production environments.

#### Microsoft Authentication Library (MSAL):
- **Usage:** For advanced scenarios where the Azure Identity client library is not sufficient.
- **Resource ID:** Required to acquire a token, specific to the cloud environment (e.g., `account-name.blob.core.windows.net` for Azure Global).

#### Assigning Azure Roles:
- **Azure RBAC Roles for Blob Data:**
  - **Storage Blob Data Owner:** Full access including managing POSIX access control.
  - **Storage Blob Data Contributor:** Read/write/delete permissions.
  - **Storage Blob Data Reader:** Read-only permissions.
  - **Storage Blob Delegator:** For creating user delegation SAS.

- **Role Scope:** Best practice is to grant the narrowest possible scope, such as individual containers, storage accounts, resource groups, subscriptions, or management groups.

#### Enabling Fine-Grained Access:
- **Azure ABAC:** Allows configuring conditions on role
## Select and configure an appropriate method for access to Azure Tables
#### Overview:
- **Microsoft Entra ID**: Used for authorizing requests to Azure table data.
- **OAuth 2.0 Token**: Obtained via Microsoft Entra ID to authorize requests.

#### Advantages:
- **Superior Security**: Better than Shared Key authorization.
- **Ease of Use**: Simplifies permission management.
- **Microsoft Recommendation**: Use Entra ID for table applications to ensure minimal required privileges.

#### Supported Environments:
- Available for all general-purpose uses in public regions and national clouds.
- Requires storage accounts created with Azure Resource Manager deployment model.

#### Authorization Process:
1. **Authentication**:
   - Security principal is authenticated, returns OAuth 2.0 token.
   - Managed identities can be used for applications within Azure (e.g., Azure VMs, Azure Functions).
2. **Authorization**:
   - OAuth 2.0 token is used to authorize access based on assigned Azure roles.

#### Assigning Azure Roles:
- **Azure RBAC Roles**:
  - **Storage Table Data Contributor**: Read/write/delete permissions.
  - **Storage Table Data Reader**: Read-only permissions.
- **Scope of Roles**:
  - Individual table, storage account, resource group, subscription, management group.
- **Best Practice**: Grant narrowest possible scope.

#### Authentication Libraries:
- **Azure Identity Client Library**: Simplifies acquiring OAuth 2.0 tokens.
- **Microsoft Authentication Library (MSAL)**: For advanced scenarios.

#### Resources for Various Languages:
- **.NET**: 
  - Authentication guides, using service principals, developer accounts, Azure-hosted apps, on-premises apps.
  - Azure Identity client library.
- **Java**: 
  - Authentication guides, using service principals, user credentials, Azure-hosted apps.
  - Azure Identity client library.
- **JavaScript**:
  - Authentication guides, using service principals, developer accounts, Azure-hosted apps, on-premises apps.
  - Azure Identity client library.
- **Python**:
  - Authentication guides, using service principals, developer accounts, Azure-hosted apps, on-premises apps.
  - Azure Identity client library.
- **Go**:
  - Service principal authentication, Azure-hosted apps.
  - Azure Identity client library.

#### Role Assignment and Permissions:
- Assign roles using Azure portal or CLI.
- Only specific data access roles (e.g., Storage Table Data Contributor, Storage Table Data Reader) allow access to table data.
- Management roles (e.g., Owner, Contributor) do not grant data access via Entra ID unless list keys action is included.
- **Propagation Delay**: Role assignments may take up to 30 minutes to propagate.

#### Permissions for Data Operations:
- Detailed permissions required for Table service operations can be found in Azure documentation.
## Select and configure an appropriate method for access to Azure Queues
#### Overview:
- **Microsoft Entra ID**: Used for authorizing requests to Azure queue data.
- **OAuth 2.0 Token**: Obtained via Microsoft Entra ID to authorize requests.

#### Advantages:
- **Superior Security**: Better than Shared Key authorization.
- **Ease of Use**: Simplifies permission management.
- **Microsoft Recommendation**: Use Entra ID for queue applications to ensure minimal required privileges.

#### Supported Environments:
- Available for all general-purpose storage accounts in public regions and national clouds.
- Requires storage accounts created with Azure Resource Manager deployment model.

#### Authorization Process:
1. **Authentication**:
   - Security principal is authenticated, returns OAuth 2.0 token.
   - Managed identities can be used for applications within Azure (e.g., Azure VMs, Azure Functions).
2. **Authorization**:
   - OAuth 2.0 token is used to authorize access based on assigned Azure roles.

#### Assigning Azure Roles:
- **Azure RBAC Roles**:
  - **Storage Queue Data Contributor**: Read/write/delete permissions.
  - **Storage Queue Data Reader**: Read-only permissions.
  - **Storage Queue Data Message Processor**: Peek, retrieve, delete message permissions.
  - **Storage Queue Data Message Sender**: Add message permissions.
- **Scope of Roles**:
  - Individual queue, storage account, resource group, subscription, management group.
- **Best Practice**: Grant narrowest possible scope.

#### Authentication Libraries:
- **Azure Identity Client Library**: Simplifies acquiring OAuth 2.0 tokens.
- **Microsoft Authentication Library (MSAL)**: For advanced scenarios.

#### Resources for Various Languages:
- **.NET**: 
  - Authentication guides, using service principals, developer accounts, Azure-hosted apps, on-premises apps.
  - Azure Identity client library.
- **Java**: 
  - Authentication guides, using service principals, user credentials, Azure-hosted apps.
  - Azure Identity client library.
- **JavaScript**:
  - Authentication guides, using service principals, developer accounts, Azure-hosted apps, on-premises apps.
  - Azure Identity client library.
- **Python**:
  - Authentication guides, using service principals, developer accounts, Azure-hosted apps, on-premises apps.
  - Azure Identity client library.
- **Go**:
  - Service principal authentication, Azure-hosted apps.
  - Azure Identity client library.

#### Role Assignment and Permissions:
- Assign roles using Azure portal, PowerShell, or CLI.
- Specific data access roles (e.g., Storage Queue Data Contributor, Storage Queue Data Reader) allow access to queue data.
- Management roles (e.g., Owner, Contributor) do not grant data access via Entra ID unless list keys action is included.
- **Propagation Delay**: Role assignments may take up to 30 minutes to propagate.

#### Permissions for Data Operations:
- Detailed permissions required for Queue service operations can be found in Azure documentation.

#### Access Data with a Microsoft Entra Account:
- **Azure Portal**: Uses either Microsoft Entra account or account access keys.
- **PowerShell/Azure CLI**: Supports signing in with Microsoft Entra credentials.

#### Best Practices:
- **Disable Shared Key Authorization**: For optimal security, use Microsoft Entra ID or SAS.
- **Azure Roles for Portal Access**: Assign Azure Resource Manager role like Reader for portal access.
## Select and configure appropriate methods for protecting against data security threats, including soft delete, backups, versioning, and immutable storage
#### Overview:
- Azure Storage offers various methods for protecting against data loss or compromise.
- It's essential to implement appropriate measures before incidents occur.

#### Basic Data Protection Recommendations:
1. **Azure Resource Manager Lock**:
   - Prevents storage account deletion or configuration changes.
2. **Container Soft Delete**:
   - Recovers deleted containers and contents.
3. **Blob Versioning**:
   - Automatically saves blob state with each overwrite (Blob Storage).
   - Manual snapshots for Azure Data Lake Storage.

#### Data Protection Options:
- **Preventing Deletion or Modification**:
  - Azure Resource Manager Lock: Protects storage account but not containers or blobs.
- **Immutability Policies**:
  - Blob Version: Protects individual blob versions.
  - Container: Protects entire container and blobs within it.
- **Soft Deletion**:
  - Container Soft Delete: Restores deleted containers.
  - Blob Soft Delete: Restores deleted blobs.
- **Version Control**:
  - Blob Versioning: Saves previous versions of blobs.
- **Recovery**:
  - Point-in-Time Restore: Reverts block blobs to earlier states.
  - Blob Snapshot: Manually saves blob states.
- **Custom Solutions**:
  - Copying Data to a Second Account: Redundancy against intentional or unpredictable scenarios.

#### Data Protection by Resource Type:
- Each option protects different levels of resources (account, container, blob).
- Understand nuances for effective implementation.

#### Recovering Deleted or Overwritten Data:
- Various recovery actions based on enabled protection options.
- Requirements for each recovery action must be met.

#### Cost Considerations:
- Different protection options have varying cost implications.
- Understand cost factors for informed decision-making.
- Consider lifecycle management to control costs.

#### Disaster Recovery:
- Azure Storage ensures data redundancy across multiple copies.
- Geo-redundancy offers failover options for disaster recovery.
## Configure Bring your own key (BYOK)
#### Scenario:
A Key Vault customer wants to securely transfer a key from their on-premises Hardware Security Module (HSM) to Azure Key Vault. This process, known as Bring Your Own Key (BYOK), ensures the key never exists outside an HSM in plain text form.

#### Requirements:
- The key to be transferred is always protected by a key held in the Azure Key Vault HSM.
- Key Exchange Key (KEK) and Wrapping Key are involved in the process.
- KEK must be an RSA-HSM key with specific properties.

#### User Steps for Key Transfer:
1. **Generate KEK**: Use Azure CLI to create a KEK with import operations.
2. **Retrieve Public Key**: Download the public key portion of KEK.
3. **Generate Key Transfer Blob**: Use the HSM vendor-provided BYOK tool to create a key transfer blob.
   - The target key plaintext is transformed using CKM_RSA_AES_KEY_WRAP mechanism.
   - The format of the transfer blob uses JSON Web Encryption compact serialization.

#### HSM Constraints:
- Existing HSM may apply constraints on managed keys.
- Configuration steps for the source HSM are usually provided by the HSM vendor.

#### Additional Considerations:
- Services support different KEK lengths.
- Various interfaces like Azure PowerShell and Azure portal can perform these steps.
- Long-term goal: Use PKCS#11 CKM_RSA_AES_KEY_WRAP mechanism for key transfer.

#### Implementation Commands (Azure CLI):
1. **Generate KEK**:
   ```
   az keyvault key create --kty RSA-HSM --size 4096 --name KEKforBYOK --ops import --vault-name ContosoKeyVaultHSM
   ```
2. **Retrieve Public Key**:
   ```
   az keyvault key download --name KEKforBYOK --vault-name ContosoKeyVaultHSM --file KEKforBYOK.publickey.pem
   ```

#### Key Transfer Blob:
- Created using HSM vendor-provided BYOK tool.
- Placed in a ".byok" file format.
- Involves encryption using CKM_RSA_AES_KEY_WRAP mechanism.
## Enable double encryption at the Azure Storage infrastructure level
#### Overview:
Azure Storage automatically encrypts all data at the service level using 256-bit AES encryption. However, for customers requiring higher levels of security assurance, enabling double encryption at the Azure Storage infrastructure level provides an additional layer of protection. This double encryption ensures data remains secure even if one encryption algorithm or key is compromised.

#### Key Points:
- Double encryption involves encrypting data twice: once at the service level and once at the infrastructure level.
- Two different encryption algorithms and keys are used for each layer of encryption.
- Infrastructure encryption is recommended for compliance requirements.
- Infrastructure encryption cannot be enabled or disabled after the storage account is created.

#### Implementation Steps:
1. **Create a Storage Account with Infrastructure Encryption Enabled**:
   - Navigate to the Azure portal > Storage accounts.
   - Click 'Add' to create a new general-purpose v2 or premium block blob storage account.
   - Under the Encryption tab, enable infrastructure encryption.
   - Review and create the storage account.

2. **Verify Infrastructure Encryption**:
   - Navigate to the storage account in the Azure portal.
   - Under Settings, select Encryption to verify that infrastructure encryption is enabled.

#### Policy Enforcement:
- Azure Policy provides a built-in policy to enforce enabling infrastructure encryption for storage accounts.

#### Considerations:
- Infrastructure encryption is recommended for compliance scenarios.
- For most scenarios, Azure Storage encryption provides sufficient security without the need for infrastructure encryption.


# Plan and implement security for Azure SQL Database and Azure SQL Managed Instance

## Enable Microsoft Entra database authentication

Enabling database authentication through Microsoft Entra ID (formerly known as Azure AD) provides a secure and efficient method to authenticate users to Azure SQL Databases, SQL Managed Instances, and Azure Synapse Analytics. This approach leverages Microsoft Entra ID to manage authentication and integrate seamlessly with existing directory services.

#### Key Concepts:
1. **Authentication vs. Authorization**:
   - **Authentication**: Verifying the identity of the user trying to access the database.
   - **Authorization**: Determining what actions the authenticated user is allowed to perform on the database.

2. **Authentication Methods**:
   - **SQL Authentication**: Users provide a username and password stored in the SQL database.
   - **Microsoft Entra ID Authentication**: Users provide credentials managed by Microsoft Entra ID, offering a more secure and centralized approach.

3. **Logins and Users**:
   - **Logins**: Stored in the master database, these are individual accounts that can link to multiple user accounts across databases.
   - **User Accounts**: Can exist in any database and may or may not be linked to a login. For contained database users, credentials are stored within the database itself, enhancing portability and scalability.

4. **Database Roles and Permissions**:
   - Roles and explicit permissions control user actions and access to data.
   - Best practice: Grant the least privileges necessary and use dedicated accounts for applications to minimize security risks.

#### Steps to Enable Microsoft Entra ID Authentication:

1. **Create a Microsoft Entra ID Admin for the SQL Server**:
   - Assign a Microsoft Entra ID user or group as an admin to the SQL server.
   - This can be done via the Azure portal, Azure CLI, or PowerShell.

   **Azure Portal**:
   - Navigate to your SQL server.
   - Under Settings, select Active Directory admin.
   - Click Set admin, and choose the desired Microsoft Entra ID user or group.

   **Azure CLI**:
   ```bash
   az sql server ad-admin create --resource-group <ResourceGroupName> --server-name <ServerName> --display-name <AdminDisplayName> --object-id <AdminObjectID>
   ```

2. **Create Contained Database Users in Azure SQL**:
   - These users are created directly in the database and authenticated via Microsoft Entra ID, without needing a login in the master database.

   **SQL Command**:
   ```sql
   CREATE USER [username] FROM EXTERNAL PROVIDER;
   ```

3. **Connect to the Database Using Microsoft Entra ID**:
   - Users can connect to the database using tools like SQL Server Management Studio (SSMS) or Azure Data Studio by choosing the "Azure Active Directory - Universal with MFA support" authentication method.

   **Example Connection String**:
   ```csharp
   string connectionString = "Server=tcp:<server>.database.windows.net,1433;Database=<database>;Authentication='Active Directory Interactive';";
   ```

4. **Verify Microsoft Entra ID Integration**:
   - Confirm that users can log in and are authenticated through Microsoft Entra ID.
   - Check user roles and permissions to ensure they align with security policies.

#### Managing and Configuring Microsoft Entra ID Admins:

1. **Resetting Server Admin Password**:
   - Use the Azure portal to reset the password for the Server admin.
   - Navigate to the SQL server or managed instance, and select Reset password under the relevant settings.

   **PowerShell**:
   ```powershell
   Set-AzSqlServer -ResourceGroupName <ResourceGroupName> -ServerName <ServerName> -SqlAdministratorPassword <NewPassword>
   ```

2. **Changing Microsoft Entra ID Admin**:
   - You can change the Microsoft Entra ID admin as required via the Azure portal or CLI.

   **Azure CLI**:
   ```bash
   az sql server ad-admin update --resource-group <ResourceGroupName> --server-name <ServerName> --display-name <NewAdminDisplayName> --object-id <NewAdminObjectID>
   ```

#### Key Considerations:

- **Security**: Microsoft Entra ID authentication simplifies credential management and enhances security by leveraging centralized identity management.
- **Scalability**: Contained database users make it easier to move databases between servers and manage user access.
- **Compliance**: Microsoft Entra ID supports multifactor authentication (MFA), providing compliance with various regulatory requirements.

Enabling Microsoft Entra ID authentication for your Azure SQL Databases is a best practice for secure and efficient database access management, especially in environments requiring centralized identity services and compliance with strict security policies.
## Enable database auditing

Enabling auditing for Azure SQL Database and Azure Synapse Analytics helps you maintain an audit trail of database activities, facilitating compliance and security monitoring. Here's a detailed guide to understand and enable database auditing in Azure.

#### Key Concepts:

1. **Purpose of Auditing**:
   - **Regulatory Compliance**: Helps in meeting compliance standards by tracking database activities.
   - **Activity Monitoring**: Provides insight into database actions and events, helping to identify discrepancies or suspicious activities.
   - **Security and Governance**: Enables monitoring of database activities to detect potential security violations.

2. **Auditing Destinations**:
   - **Azure Storage Account**: Logs are stored as `.xel` files in Blob Storage.
   - **Log Analytics Workspace**: Integrates with Azure Monitor for advanced query and analysis.
   - **Event Hubs**: Facilitates streaming of audit logs for real-time processing.

3. **Audit Configuration Options**:
   - **Database-Level Auditing**: Configures auditing for individual databases.
   - **Server-Level Auditing**: Configures auditing for all databases on a server.
   - **Audit Action Groups**: Specifies categories of actions to be logged, such as successful logins or data modifications.

4. **Auditing Limitations**:
   - **Paused Synapse SQL Pools**: Auditing cannot be enabled on paused pools.
   - **User Assigned Managed Identity (UAMI)**: Not supported for Azure Synapse.
   - **Default Audit Action Groups**: Only these groups are supported for Synapse SQL pools.

#### Steps to Enable Database Auditing:

1. **Enable Auditing via Azure Portal**:
   - Navigate to the Azure portal and open your SQL server or database.
   - Under **Security**, select **Auditing**.
   - Turn **Auditing** on, and configure the desired audit destination (e.g., Azure Storage, Log Analytics, Event Hubs).

   **Example Configuration**:
   - **Azure Storage Account**:
     - Choose your storage account.
     - Configure the retention period if needed.

   **Example Portal Screenshot**:
   ![Azure Portal Auditing Configuration](https://docs.microsoft.com/en-us/azure/azure-sql/database/media/audit-write-append-blob/audit-config.png)

2. **Enable Auditing via Azure CLI**:
   - Use Azure CLI to configure auditing settings programmatically.
   ```bash
   az sql db audit-policy update --name <DatabaseName> --resource-group <ResourceGroupName> --server <ServerName> --state Enabled --storage-account <StorageAccountName>
   ```

3. **Enable Auditing via PowerShell**:
   - Use PowerShell commands to enable and configure auditing.
   ```powershell
   Set-AzSqlDatabaseAudit -ResourceGroupName <ResourceGroupName> -ServerName <ServerName> -DatabaseName <DatabaseName> -StorageAccountName <StorageAccountName>
   ```

4. **Monitor Audit Logs**:
   - Access audit logs stored in Azure Storage via Azure Blob Storage Explorer or directly from the Azure portal.
   - Use Log Analytics or custom scripts to query and analyze logs for insights.
   - Use SQL Server Management Studio (SSMS) to open `.xel` files and review the audit trail.

#### Advanced Audit Configurations:

1. **Immutable Storage for Audit Logs**:
   - Configure audit logs to be written to immutable blob storage to prevent tampering.
   - Ensure you select **Allow additional appends** when setting up immutable storage.
   - [Immutable Blob Storage Configuration](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blob-immutable-storage-configure)

2. **Write Audit Logs to a Storage Account Behind a VNet or Firewall**:
   - Upgrade to a general-purpose v2 storage account if using VNet or firewall restrictions.
   - Ensure the storage account is configured correctly to allow audit logging.
   - [Configure Storage Account for Audit Logs](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-auditing#write-audit-logs-to-a-storage-account-behind-a-vnet-or-firewall)

3. **Threat Detection and Alerts**:
   - Enable the threat detection feature to receive alerts on suspicious database activities.
   - Configure email notifications for real-time alerts.
   - [Configure Threat Detection](https://docs.microsoft.com/en-us/azure/azure-sql/database/threat-detection-configure)

4. **Handling Failed Logins with Microsoft Entra ID**:
   - Failed Microsoft Entra logins are not logged in SQL audit logs but can be monitored through the Microsoft Entra admin center.
   - Successful logins are audited as they reach the database and can be analyzed for suspicious activity.

#### Key Considerations:

- **Storage Account Compatibility**: Ensure your storage account type supports the auditing setup, especially when using advanced features like VNet or firewall.
- **Read-Only Replicas**: Auditing is automatically enabled for read-only replicas, providing comprehensive tracking of read-only operations.
- **High Activity Periods**: During high load times, some audit events may not be logged to prioritize database performance.

By enabling and monitoring database auditing, you can enhance your security posture, ensure compliance, and gain valuable insights into your database activities in Azure SQL Database and Azure Synapse Analytics.


## Identify use cases for the Microsoft Purview governance portal

### Core Use Cases & Features

1. **Holistic Data Management**:
   - **Automated Discovery**: Scans and catalogs data assets across your data estate.
   - **Sensitive Data Classification**: Identifies and classifies sensitive data.
   - **Data Lineage Tracking**: Maps the flow of data from source to consumption.

2. **Data Governance and Security**:
   - **Curator Tools**: Manage metadata and ensure data quality.
   - **Security Administration**: Secure data access with robust policies.
   - **Unified Data Map**: Integrates metadata for a comprehensive data landscape view.

3. **Empowering Data Consumers**:
   - **Data Catalog**: Searchable catalog enriched with metadata for easy data discovery.
   - **Lineage Visualization**: Shows data flow and transformation across systems.

---

### Detailed Applications

1. **Data Map**:
   - **Discovery & Classification**: Automatically scans and updates metadata.
   - **PaaS Service**: Provides scalable, cloud-native data governance.

2. **Data Catalog**:
   - **Search & Discovery**: Easily find and access data with rich metadata filters.
   - **Glossary & Tagging**: Automate tagging and manage glossary terms.
   - **Lineage**: Visualize data’s journey and transformations.

3. **Data Estate Insights**:
   - **Governance Overview**: High-level view of data estate and governance gaps.
   - **Actionable Insights**: Directly address governance issues.

4. **Data Sharing**:
   - **Secure Sharing**: Manage data sharing within and outside the organization.
   - **Central Monitoring**: Control and monitor shared data access.

5. **Data Policy**:
   - **Centralized Management**: Unified platform for managing data access policies.
   - **Fine-Grained Control**: Role-based permissions and metadata-driven policies.
   - **Scalability**: Supports SaaS, on-premises, and multicloud sources.

---

### How to Use:

1. **Access Portal**: Via Azure, navigate to Microsoft Purview.
2. **Configure Data Map**: Set up automated scanning and metadata integration.
3. **Use Data Catalog**: Search and discover data assets, visualize lineage.
4. **Analyze Insights**: Use dashboards to monitor data governance.
5. **Manage Sharing**: Securely share and monitor data access.
6. **Apply Policies**: Define access rules and ensure compliance.

---

### Key Points:

- **Compliance & Security**: Enhance data security and compliance.
- **Data Governance**: Mature governance practices from discovery to policy.
- **Integration**: Use APIs for seamless integration with existing systems.

Microsoft Purview is essential for efficient, compliant, and secure data management across diverse environments.
## Implement data classification of sensitive information by using the Microsoft Purview governance portal

### Key Steps in Data Classification

1. **Register and Manage Data Sources**:
   - **Prerequisites**: Ensure you have a Microsoft Purview account and necessary roles (Data Source Admin, Data Reader, etc.).
   - **Register Source**: Provide Purview with the location and details of your data source.
   - **Configure Connection**: Set up network and authentication details for secure access.

2. **Scan and Ingest Data**:
   - **Scanning**: Capture metadata from data sources (e.g., file names, schemas).
   - **Ingestion**: Process and store metadata in the Purview Data Map and Data Catalog.
   - **Customization**: Use scan rule sets to target specific data types or areas for efficient scanning.

3. **Classify Data**:
   - **System Classifications**: Use built-in tags for common data types like "Credit Card Number" or "Passport Number."
   - **Custom Classifications**: Create rules for unique data patterns (e.g., custom employee IDs).
   - **Automated Classification**: Apply classifications during the scanning process based on predefined rules.

4. **Label Data for Security**:
   - **Sensitivity Labels**: Tag data based on its security level (e.g., "Highly Confidential").
   - **Automatic Labeling**: Use rules to auto-assign labels during scans based on the data's sensitivity.
   - **Label Application**: Apply labels to both files in storage and columns in databases.

---

### Detailed Applications

1. **Data Map**:
   - **Automated Scanning**: Capture and update metadata regularly.
   - **Central Metadata Repository**: Provides a comprehensive view of data assets.

2. **Data Catalog**:
   - **Search and Discover**: Use detailed filters to find and manage data assets.
   - **Lineage and Governance**: Trace data origins and transformations.

3. **Classification**:
   - **Organize Data**: Apply logical categories to simplify data management.
   - **Risk Management**: Identify and secure sensitive data.

4. **Labeling**:
   - **Travel with Data**: Labels persist across platforms and applications.
   - **Security Insights**: Enhance data protection and compliance reporting.
   - **Multi-Platform Support**: Apply labels across Azure Data Lake, SQL databases, and more.

---

### How to Use:

1. **Set Up Data Sources**:
   - Register and configure sources with appropriate network and security settings.

2. **Execute Scans**:
   - Schedule and customize scans to capture necessary metadata.
   - Use scan rule sets for efficient data ingestion.

3. **Classify and Label**:
   - Apply both system and custom classifications during scans.
   - Define sensitivity labels and auto-labeling rules for consistent security tagging.

4. **Manage and Monitor**:
   - Use the Purview portal to continuously oversee data governance.
   - Regularly update and refine classification and labeling rules to adapt to changing data landscapes.

---

### Key Points:

- **Comprehensive Governance**: From registration to labeling, Microsoft Purview offers a unified approach to data governance.
- **Automation**: Leverage automated tools for classification and labeling to streamline data management.
- **Security and Compliance**: Ensure your data meets security and regulatory standards with sensitivity labels and robust metadata management.

Microsoft Purview simplifies the process of managing and protecting sensitive data, making it a crucial tool for maintaining data integrity and compliance across your organization.
## Plan and implement dynamic masking

**Objective:**
Dynamic data masking (DDM) in Azure SQL Database, Azure SQL Managed Instance, and Azure Synapse Analytics helps to limit sensitive data exposure by masking it to non-privileged users. This feature allows you to control how much of the sensitive data is visible to different users while ensuring minimal impact on the application layer.

**Scenario:**
Consider a call center where a service representative needs to confirm part of a caller's email address for identification purposes. DDM can mask the email address so that only a portion is revealed, protecting the full email address from unauthorized access.

### Steps to Implement Dynamic Data Masking

1. **Access Dynamic Data Masking in Azure Portal:**
   - Navigate to the Azure SQL Database instance.
   - Select the **Dynamic Data Masking** blade under the **Security** section.

2. **Set Up Dynamic Data Masking Policy:**
   - **SQL Users Excluded from Masking:** Define a set of SQL users or Microsoft Entra ID identities that will always see unmasked data. Administrators are automatically excluded from masking.
   - **Masking Rules:** Create rules that specify which fields to mask and how. Rules can be applied based on the schema, table, and column names.

3. **Choose Masking Functions:**
   - **Default:** Masks the full value based on the data type. For instance, strings are masked with 'XXXX', numeric values are masked with zero, and dates are masked with '1900-01-01'.
   - **Email:** Masks all but the first letter of the email and appends a generic domain, like `aXXX@XXXX.com`.
   - **Random:** Generates a random number within a specified range for numeric fields.
   - **Custom String:** Masks part of the string and replaces it with a custom padding. For example, a phone number like `555.123.1234` could be masked to `5XXXXXXX`.
   - **Datetime:** Masks parts of datetime values such as the year, month, or minute.

   Example: To mask a phone number while keeping the first digit visible, use:
   ```sql
   ALTER COLUMN [Phone Number] ADD MASKED WITH (FUNCTION = 'partial(1,"XXXXXXX",0)');
   ```

4. **Recommended Fields to Mask:**
   - Use the DDM recommendations engine to identify sensitive fields in your database that are good candidates for masking. Apply the appropriate masking functions to these fields through the portal interface.

### Managing Dynamic Data Masking Using T-SQL

- **Create a Dynamic Data Mask:** Define a mask on a column.
  ```sql
  ALTER TABLE Employees ALTER COLUMN SSN ADD MASKED WITH (FUNCTION = 'default()');
  ```

- **Add or Edit a Mask on an Existing Column:** Modify the existing masking rule on a column.
  ```sql
  ALTER TABLE Employees ALTER COLUMN Email ADD MASKED WITH (FUNCTION = 'email()');
  ```

- **Grant Permissions to View Unmasked Data:** Assign `UNMASK` permissions to specific users to allow them to view unmasked data.
  ```sql
  GRANT UNMASK ON TABLE Employees TO db_user;
  ```

- **Drop a Dynamic Data Mask:** Remove a masking rule from a column.
  ```sql
  ALTER TABLE Employees ALTER COLUMN SSN DROP MASKED;
  ```

### Managing Dynamic Data Masking Using PowerShell

- **Data Masking Policies:**
  - Retrieve: `Get-AzSqlDatabaseDataMaskingPolicy`
  - Set: `Set-AzSqlDatabaseDataMaskingPolicy`

- **Data Masking Rules:**
  - Retrieve: `Get-AzSqlDatabaseDataMaskingRule`
  - Create: `New-AzSqlDatabaseDataMaskingRule`
  - Remove: `Remove-AzSqlDatabaseDataMaskingRule`
  - Set: `Set-AzSqlDatabaseDataMaskingRule`

### Managing Dynamic Data Masking Using REST API

- **Data Masking Policies:**
  - Create or Update a policy: `Create Or Update`
  - Get a policy: `Get`

- **Data Masking Rules:**
  - Create or Update a rule: `Create Or Update`
  - List rules by database: `List By Database`

### Permissions and Granular Control

To implement DDM, you need appropriate permissions. These include:

- **Roles:**
  - SQL Security Manager
  - SQL DB Contributor
  - SQL Server Contributor

- **Actions:**
  - Read/Write: `Microsoft.Sql/servers/databases/dataMaskingPolicies/*`
  - Read: `Microsoft.Sql/servers/databases/dataMaskingPolicies/read`
  - Write: `Microsoft.Sql/servers/databases/dataMaskingPolicies/write`

**Granular Permissions:**
You can control access to unmasked data at various levels:
- Database-level
- Schema-level
- Table-level
- Column-level

**Example:**
To grant a user access to unmasked data for a specific column:
```sql
GRANT UNMASK ON Data.Membership(FirstName) TO ServiceAttendant;
```

### SQL Examples: Applying and Managing Dynamic Data Masking

1. **Create Schema and Table with Masked Columns:**
   ```sql
   CREATE SCHEMA Data;
   GO
   CREATE TABLE Data.Membership (
       MemberID INT IDENTITY(1, 1) NOT NULL PRIMARY KEY,
       FirstName VARCHAR(100) MASKED WITH (FUNCTION = 'partial(1, "xxxxx", 1)') NULL,
       LastName VARCHAR(100) NOT NULL,
       Phone VARCHAR(12) MASKED WITH (FUNCTION = 'default()') NULL,
       Email VARCHAR(100) MASKED WITH (FUNCTION = 'email()') NOT NULL,
       DiscountCode SMALLINT MASKED WITH (FUNCTION = 'random(1, 100)') NULL,
       BirthDay DATETIME MASKED WITH (FUNCTION = 'default()') NULL
   );
   ```

2. **Insert Sample Data:**
   ```sql
   INSERT INTO Data.Membership (FirstName, LastName, Phone, Email, DiscountCode, BirthDay)
   VALUES
   ('Roberto', 'Tamburello', '555.123.4567', 'RTamburello@contoso.com', 10, '1985-01-25 03:25:05'),
   ('Janice', 'Galvin', '555.123.4568', 'JGalvin@contoso.com.co', 5, '1990-05-14 11:30:00'),
   ('Shakti', 'Menon', '555.123.4570', 'SMenon@contoso.net', 50, '2004-02-29 14:20:10'),
   ('Zheng', 'Mu', '555.123.4569', 'ZMu@contoso.net', 40, '1990-03-01 06:00:00');
   ```

3. **Create Service Table with Masked Columns:**
   ```sql
   CREATE SCHEMA Service;
   GO
   CREATE TABLE Service.Feedback (
       MemberID INT IDENTITY(1, 1) NOT NULL PRIMARY KEY,
       Feedback VARCHAR(100) MASKED WITH (FUNCTION = 'default()') NULL,
       Rating INT MASKED WITH (FUNCTION = 'default()'),
       Received_On DATETIME
   );
   ```

4. **Insert Sample Data into Service Table:**
   ```sql
   INSERT INTO Service.Feedback (Feedback, Rating, Received_On)
   VALUES
   ('Good', 4, '2022-01-25 11:25:05'),
   ('Excellent', 5, '2021-12-22 08:10:07'),
   ('Average', 3, '2021-09-15 09:00:00');
   ```

5. **Create Users and Assign Read Permissions:**
   ```sql
   CREATE USER ServiceAttendant WITHOUT LOGIN;
   CREATE USER ServiceLead WITHOUT LOGIN;
   CREATE USER ServiceManager WITHOUT LOGIN;
   CREATE USER ServiceHead WITHOUT LOGIN;
   ALTER ROLE db_datareader ADD MEMBER ServiceAttendant;
   ALTER ROLE db_datareader ADD MEMBER ServiceLead;
   ALTER ROLE db_datareader ADD MEMBER ServiceManager;
   ALTER ROLE db_datareader ADD MEMBER ServiceHead;
   ```

6. **Grant UNMASK Permissions:**
   ```sql
   -- Column-level
   GRANT UNMASK ON Data.Membership(FirstName) TO ServiceAttendant;

   -- Table-level
   GRANT UNMASK ON Data.Membership TO ServiceLead;

   -- Schema-level
   GRANT UNMASK ON SCHEMA::Data TO ServiceManager;
   GRANT UNMASK ON SCHEMA::Service TO ServiceManager;

   -- Database-level
   GRANT UNMASK TO ServiceHead;
   ```

7. **Query Data as Different Users:**
   ```sql
   -- Query as ServiceAttendant
   EXECUTE AS USER = 'ServiceAttendant';
   SELECT MemberID, FirstName, LastName, Phone, Email, BirthDay FROM Data.Membership;
   SELECT MemberID, Feedback, Rating FROM Service.Feedback;
   REVERT;

   -- Query as ServiceLead
   EXECUTE AS USER = 'ServiceLead';
   SELECT MemberID, FirstName, LastName, Phone, Email, BirthDay FROM Data.Membership;
   SELECT MemberID, Feedback, Rating FROM Service.Feedback;
   REVERT;

   -- Query as ServiceManager
   EXECUTE AS USER = 'ServiceManager';
   SELECT MemberID, FirstName, LastName, Phone, Email, BirthDay FROM Data.Membership;
   SELECT MemberID, Feedback, Rating FROM Service.Feedback;
   REVERT;

   -- Query as ServiceHead
   EXECUTE AS USER = 'Service

Head';
   SELECT MemberID, FirstName, LastName, Phone, Email, BirthDay FROM Data.Membership;
   SELECT MemberID, Feedback, Rating FROM Service.Feedback;
   REVERT;
   ```

8. **Revoke UNMASK Permissions:**
   ```sql
   REVOKE UNMASK ON Data.Membership(FirstName) FROM ServiceAttendant;
   REVOKE UNMASK ON Data.Membership FROM ServiceLead;
   REVOKE UNMASK ON SCHEMA::Data FROM ServiceManager;
   REVOKE UNMASK ON SCHEMA::Service FROM ServiceManager;
   REVOKE UNMASK FROM ServiceHead;
   ```

### Summary

Dynamic Data Masking (DDM) is a powerful feature to protect sensitive data in Azure SQL databases by masking it from unauthorized users. You can configure DDM through the Azure portal, T-SQL, PowerShell, or REST API, providing flexible and granular control over data visibility.

This example illustrated how to set up and manage DDM for different scenarios, ensuring compliance and data security while maintaining operational functionality.
## Implement Transparent Data Encryption (TDE)

### Overview
- **TDE Purpose**: Encrypts data at rest to protect against unauthorized access and malicious offline activity.
- **Real-time Encryption**: Encrypts and decrypts data as it is written to and read from the database.
- **Default Encryption**: Enabled by default for all new Azure SQL databases.

### Core Concepts
1. **Database Encryption Key (DEK)**
   - A symmetric key used for encryption and decryption of the database.
   - Protected by a TDE protector, which can be either a service-managed certificate or a customer-managed key stored in Azure Key Vault.

2. **Encryption Algorithms**
   - Uses AES 256 encryption algorithm.
   - The DEK encrypts database storage, backups, and transaction log files.

3. **TDE Protector**
   - **Service-Managed**: Managed by Azure, a built-in certificate encrypts the DEK.
   - **Customer-Managed (BYOK)**: An asymmetric key in Azure Key Vault, managed by the customer.

### Enabling TDE
- **Service-Managed TDE**
  - **Azure Portal**: Navigate to your database, select **Transparent Data Encryption**, and enable it.
  - **T-SQL**:
    ```sql
    CREATE DATABASE ENCRYPTION KEY
    WITH ALGORITHM = AES_256
    ENCRYPTION BY SERVER CERTIFICATE [ServerCertificateName];
    ALTER DATABASE [YourDatabaseName] SET ENCRYPTION ON;
    ```
  - **PowerShell**:
    ```powershell
    Set-AzSqlDatabaseTransparentDataEncryption -ResourceGroupName "YourResourceGroup" -ServerName "YourServerName" -DatabaseName "YourDatabaseName" -State Enabled
    ```

- **Customer-Managed TDE (BYOK)**
  - **Azure Portal**: Configure TDE protector to use an Azure Key Vault key.
  - **T-SQL**:
    ```sql
    CREATE DATABASE ENCRYPTION KEY
    WITH ALGORITHM = AES_256
    ENCRYPTION BY SERVER ASYMMETRIC KEY [YourAsymmetricKeyName];
    ALTER DATABASE [YourDatabaseName] SET ENCRYPTION ON;
    ```
  - **PowerShell**:
    ```powershell
    Set-AzSqlServerTransparentDataEncryptionProtector -ResourceGroupName "YourResourceGroup" -ServerName "YourServerName" -Type AzureKeyVault -KeyVaultKeyUri "https://YourKeyVaultName.vault.azure.net/keys/YourKeyName"
    ```

### Managing TDE
- **Check Encryption Status**:
  ```sql
  SELECT db_name(database_id) as DatabaseName, encryption_state
  FROM sys.dm_database_encryption_keys;
  ```
- **Disable TDE**:
  ```sql
  ALTER DATABASE [YourDatabaseName] SET ENCRYPTION OFF;
  ```

### Special Considerations
- **Geo-Replication and Backups**: TDE settings are inherited by the target database during geo-replication or restore operations.
- **Exporting Databases**: BACPAC files exported from TDE-protected databases are not encrypted.
- **System Databases**: TDE cannot encrypt system databases (e.g., master, tempdb).

### Monitoring and Auditing
- **Azure Portal**: Regularly monitor TDE settings under the database's TDE configuration.
- **Azure Key Vault Logging**: Track access and usage of encryption keys.

### Summary
- TDE is critical for securing data at rest in Azure SQL environments.
- Choose between service-managed or customer-managed keys based on your control and compliance requirements.
- Regularly monitor and audit TDE to maintain security and compliance.
## Recommend when to use Azure SQL Database Always Encrypted
### Overview
- **Purpose**: Protects sensitive data (e.g., credit card numbers, SSNs) in Azure SQL Database, Azure SQL Managed Instance, and SQL Server.
- **Data Separation**: Encrypts data inside client applications, preventing database administrators from accessing sensitive data.
- **Transparency**: Encryption and decryption occur without application changes, managed by client-side drivers.

### When to Use Always Encrypted
- **Sensitive Data Storage**: For data that must remain confidential even from database administrators (e.g., personally identifiable information (PII), financial data).
- **Compliance**: To meet regulatory requirements that mandate encryption of sensitive data at rest and in use.
- **Data Ownership Separation**: When the people managing the data should not have access to the actual data.

### Key Features
1. **Types of Encryption**:
   - **Deterministic Encryption**:
     - Same plaintext value always results in the same encrypted value.
     - Supports operations like equality comparisons, joins, and indexing.
     - Use cases: ID numbers, email addresses.
   - **Randomized Encryption**:
     - Produces different encrypted values for the same plaintext value each time.
     - More secure, but does not support search, grouping, or indexing.
     - Use cases: free-text comments, notes.

2. **Encryption Keys**:
   - **Column Encryption Key (CEK)**:
     - Encrypts data in database columns.
   - **Column Master Key (CMK)**:
     - Encrypts CEKs.
     - Stored outside the database (e.g., Azure Key Vault, Windows Certificate Store).

3. **Secure Enclaves**:
   - Extend functionality to support complex operations like pattern matching and sorting on encrypted data.
   - Allow cryptographic operations within the database using T-SQL.

### Configuration Steps
1. **Provision Keys**:
   - Generate and store CMKs in a trusted key store.
   - Create and encrypt CEKs with CMKs.
   - Store metadata about CMKs and CEKs in the database.

2. **Encrypt Columns**:
   - Define which columns to encrypt and the type of encryption to use.
   - Encrypt existing data or specify encryption for new columns.

3. **Use Client-Side Tools**:
   - SQL Server Management Studio (SSMS), SQL Server PowerShell, or `sqlpackage` for setup.
   - Client applications need Always Encrypted-enabled drivers for automatic encryption/decryption.

### Querying Encrypted Data
- **Prerequisites**:
  - Access to the CMK.
  - Database connection with Always Encrypted enabled.
- **Process**:
  - The client driver encrypts query parameters before sending them to the Database Engine.
  - The Database Engine executes the query and returns encrypted results.
  - The client driver decrypts the results before presenting them to the application.

### Limitations and Considerations
- **Deterministic Encryption**:
  - Only supports equality-based operations: `=`, `IN`, `GROUP BY`, `DISTINCT`.
  - Not suitable for columns with high-guessable values due to pattern exposure risk.

- **Randomized Encryption**:
  - Prevents operations like searching, grouping, and joining.
  - Not compatible with columns requiring frequent queries or computations.

- **Unsupported Features**:
  - Certain data types (e.g., `xml`, `timestamp`, `image`).
  - Operations involving mixed encrypted and plaintext data.
  - SQL Server replication and some distributed queries.

- **SQL Tools and Permissions**:
  - SQL tools like SSMS and Azure Data Studio require special parameterization for encrypted queries.
  - Users need `VIEW ANY COLUMN MASTER KEY DEFINITION` and `VIEW ANY COLUMN ENCRYPTION KEY DEFINITION` permissions to query encrypted columns.

### Summary
Always Encrypted in Azure SQL Database is essential for securing sensitive data by keeping it encrypted within the database and only decrypting it at the client application. This ensures a clear separation between data owners and administrators, providing robust protection against unauthorized access. By understanding and implementing the key features and limitations, you can effectively use Always Encrypted to safeguard your data.
