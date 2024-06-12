### Plan and implement advanced security for compute
1. Which feature is essential for monitoring the security of Azure Container Instances (ACIs)?

Azure Monitor

Azure Monitor provides comprehensive monitoring capabilities, allowing for the detection of security issues and performance anomalies in ACIs.

2. What enables secure remote access to Azure VMs without exposing them to the public internet?

Azure Bastion

Azure Bastion provides secure, seamless RDP/SSH access within the Azure portal over SSL, eliminating the need for public IP addresses on VMs.

3. What best practice secures access to Azure Container Registry (ACR)?

Integration with Microsoft Entra ID and RBAC

Integrating ACR with Microsoft Entra ID and using RBAC ensures that access is securely managed based on user roles and responsibilities.

4. What encryption method is recommended for securing data at rest on Azure VMs?

Azure Disk Encryption using BitLocker or dm-crypt

Azure Disk Encryption leverages BitLocker (Windows) and dm-crypt (Linux) to securely encrypt virtual machine disks, protecting data at rest.

5. How is network isolation achieved in Azure Kubernetes Service (AKS)?

Using network policies to control ingress and egress traffic

Network policies in AKS enable administrators to define rules that govern ingress and egress traffic to pods, enhancing security.
### Plan and implement security for storage
1. What is the recommended way to manage and rotate your Azure storage account access keys?

 Use Azure Key Vault to manage and rotate your keys

Correct. Azure Key Vault makes it easy to rotate your keys without interruption to your applications and provides secure management of your access keys.

2. What is the purpose of using Azure Key Vault to manage storage account access keys?

To manage and rotate existing access keys securely

Correct. Azure Key Vault provides a secure way to manage and rotate storage account access keys, which should be done regularly to maintain security.

3. What is the recommended way to authorize access to data in Azure Storage?

Using either Microsoft Entra ID or a shared access signature SAS

Correct. Microsoft recommends that clients use either Microsoft Entra ID or a shared access signature SAS to authorize access to data in Azure Storage.

4. What is the purpose of authorizing access to resources in Azure Storage?

To ensure that resources are accessible only when authorized and only to those users or applications to whom access is granted

Correct. Authorization ensures that resources are secure and only accessible to authorized users or applications.

5. What is the purpose of Bring Your Own Key (BYOK) in Azure Key Vault?

To securely transfer a key from an on-premises Hardware Security Module (HSM) outside Azure, into the HSM backing Azure Key Vault

Correct. BYOK is used to securely transfer an existing key from an on-premises HSM to Azure Key Vault for secure storage and management.

### Plan and implement security for Azure SQL Database and Azure SQL Managed Instance
1. Why is database auditing enabled in Azure SQL Database?

To track database activities and detect security violations.

Auditing enables tracking of database access and activities, helping to identify unauthorized actions and ensure compliance with security policies.

2. When is Azure SQL Database Always Encrypted recommended?

For scenarios requiring encryption of sensitive data in use, at rest, and in transit.

Always Encrypted provides a comprehensive encryption solution, protecting sensitive data throughout its lifecycle.

3. What is the benefit of implementing Transparent Database Encryption (TDE) in Azure SQL Database?

To encrypt data at rest, protecting stored data from unauthorized access.

TDE encrypts stored data, ensuring it remains inaccessible to unauthorized users without the appropriate encryption keys.

4. What is a use case for the Microsoft Purview governance portal?

To classify and manage sensitive information across data sources.

The Microsoft Purview governance portal provides tools for discovering, classifying, and managing sensitive data across an organization's data estate.

5. What is the purpose of Microsoft Entra database authentication in Azure SQL Database?

To secure database access by integrating with Microsoft Entra ID.

Microsoft Entra database authentication enhances security by allowing for Microsoft Entra ID-based authentication, providing a more secure and manageable access control mechanism.