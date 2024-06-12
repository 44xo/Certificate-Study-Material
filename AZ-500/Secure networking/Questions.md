### Plan and implement security for virtual networks
1. A company wants to configure network security as a natural extension of an application's structure. They want to group virtual machines and define network security policies based on those groups. Which Azure feature should they use?

Application Security Groups (ASGs)

Correct. ASGs enable you to configure network security as a natural extension of an application's structure, allowing you to group virtual machines and define network security policies based on those groups.

2. A company needs to connect two virtual networks in Azure so that traffic can be routed between them. Which virtual network connection type should they use?

Virtual network peering

Correct. Virtual network peering connects two Azure virtual networks so that they appear as one for connectivity purposes. Traffic between virtual machines in the peered virtual networks is routed through the Microsoft backbone infrastructure, through private IP addresses only. No public internet is involved. This provides a low latency, high bandwidth connection and is useful in scenarios such as cross-region data replication and database failover.

3. A company wants to establish network topologies that combine cross premises connectivity with inter virtual network connectivity. Which Azure feature allows them to do this?

VNet peering

Correct. VNet peering allows the establishment of network topologies that combine cross premises connectivity with inter virtual network connectivity without using a virtual network gateway.

4. How does Network Watcher help monitor network security in Azure?

By enabling monitoring of network traffic flow and identifying security rule violations.

Network Watcher helps identify and diagnose network issues, including security rule violations, by providing insights into network traffic flow and connectivity.

5. What is the primary purpose of Network Security Groups (NSGs) in Azure Virtual Networks?

To filter network traffic to and from Azure resources within an Azure Virtual Network

Correct because NSGs allow or deny network traffic to resources within a virtual network. They can be associated with either subnets or individual VM instances, providing a way to enforce and control access policies at a granular level.
### Plan and implement security for private access to Azure resources
1. What is the purpose of Azure App Service?

 To host web and mobile applications

Correct. Azure App Service is a fully managed platform for building, deploying, and scaling web and mobile applications.

2. A company wants to route traffic for their custom containers through virtual network integration. What must they ensure is configured in addition to the routing setting?

Any firewall or Network Security Group configured on traffic from the subnet allow traffic to port 443 and 445.

Correct. In addition to configuring the routing, the company must also ensure that any firewall or Network Security Group configured on traffic from the subnet allow traffic to port 443 and 445 in order to route traffic for their custom containers through virtual network integration.

3. A company wants to deploy a web application on Azure. They want to ensure that the application is highly available and can handle sudden spikes in traffic. Which of the following Azure services should they use?

Azure App Service

Correct. Azure App Service is a fully managed platform for building, deploying, and scaling web and mobile applications. It provides automatic scaling and high availability features.

4. A company wants to create a Private Link service in Azure. What is the purpose of an alias in a Private Link service?

To provide a globally unique name for the service that can be shared with customers and used to request a connection to the service.

Correct. The alias is a globally unique name for the service that can be shared with customers and used to request a connection to the service. It helps mask the customer data for the service and creates an easy-to-share name for the service.

5. A company wants to securely connect to an Azure service using a private IP address. Which of the following options should they use?

Private endpoint

Correct. A private endpoint provides a private and secure connection to an Azure service using a private IP address from the virtual network.
### Plan and implement security for public access to Azure resources
1. What is the purpose of implementing Transport Layer Security (TLS) in Azure applications?

To secure communications between clients and servers by encrypting data in transit.

TLS provides encryption for data in transit, enhancing the security of communications between clients and servers, making it essential for protecting sensitive information.

2. How does Azure Firewall contribute to the security of Azure resources?

By managing and logging all inbound and outbound network traffic according to firewall policies.

Azure Firewall centralizes the management of network traffic rules, providing enhanced logging and threat intelligence for inbound and outbound traffic.

3. What is the primary function of Azure Application Gateway?

To provide application-level routing and load balancing services.

Azure Application Gateway offers advanced routing and load balancing, optimizing web traffic delivery and increasing application security.

4. Which Azure service provides protection against Distributed Denial of Service (DDoS) attacks?

Azure DDoS Protection.

Correct because Azure DDoS Protection offers enhanced DDoS mitigation features to protect Azure resources from DDoS attacks.

5. Why integrate a Web Application Firewall (WAF) with Azure services?

To protect web applications from common web vulnerabilities and attacks.

WAF is designed to safeguard applications against web-based attacks and exploits, such as SQL injection and cross-site scripting, enhancing application security.