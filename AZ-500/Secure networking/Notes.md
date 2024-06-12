# Plan and implement security for virtual networks

## Plan and implement Network Security Groups (NSGs) and Application Security Groups (ASGs)
#### Network Security Groups (NSGs)
NSGs filter network traffic between Azure resources in a virtual network. They contain rules that allow or deny inbound/outbound network traffic.

**Key Properties of NSG Rules:**
- **Name:** Unique identifier (up to 80 characters).
- **Priority:** Number between 100 and 4096. Lower numbers have higher priority.
- **Source/Destination:** Can be an IP address, CIDR block, service tag, or ASG.
- **Protocol:** TCP, UDP, ICMP, ESP, AH, or Any.
- **Direction:** Inbound or outbound.
- **Port Range:** Specific or range of ports (e.g., 80 or 10000-10005).
- **Action:** Allow or deny.

**Important Concepts:**
- **Five-Tuple:** Rules are based on source, source port, destination, destination port, and protocol.
- **Stateful Rules:** Existing connections are maintained even if the rules are changed.
- **Association:** NSGs can be associated with subnets and network interfaces (NICs).

**Inbound Traffic Processing:**
1. Process rules in NSG associated with the subnet.
2. Process rules in NSG associated with the NIC.

**Outbound Traffic Processing:**
1. Process rules in NSG associated with the NIC.
2. Process rules in NSG associated with the subnet.

**Examples:**
- **VM1:** NSG1 (subnet) rules first, then NSG2 (NIC) rules.
- **VM2:** Only NSG1 (subnet) rules.
- **VM3:** Only NSG2 (NIC) rules, as there is no NSG for the subnet.
- **VM4:** All traffic allowed, no NSG for subnet or NIC.

**Best Practices:**
- Associate NSGs with either a subnet or NIC, not both, to avoid conflicts.
- Use Azure Network Watcher for effective rule verification and troubleshooting.

#### Application Security Groups (ASGs)
ASGs simplify network security management by grouping VMs and defining rules based on these groups.

**Features of ASGs:**
- **Grouping:** Manage rules for groups of VMs, not individual IPs.
- **Scalability:** Reuse security policies across different ASGs.

**Example Scenario:**
- **AsgWeb:** NIC1, NIC2 (Web Servers)
- **AsgLogic:** NIC3 (Application Logic)
- **AsgDb:** NIC4 (Database Servers)

**NSG1 Rules Example:**
1. **Allow-HTTP-Inbound-Internet:**
   - **Priority:** 100
   - **Source:** Internet
   - **Destination:** AsgWeb
   - **Ports:** 80 (HTTP)
   - **Action:** Allow
2. **Deny-Database-All:**
   - **Priority:** 120
   - **Source:** *
   - **Destination:** AsgDb
   - **Ports:** 1433 (SQL Server)
   - **Action:** Deny
3. **Allow-Database-BusinessLogic:**
   - **Priority:** 110
   - **Source:** AsgLogic
   - **Destination:** AsgDb
   - **Ports:** 1433 (SQL Server)
   - **Action:** Allow

**Constraints:**
- **Limits:** Number of ASGs per subscription is limited.
- **VNet Restriction:** All NICs in an ASG must be in the same VNet.
- **Rule Creation:** Source and destination ASGs must be within the same VNet for rules to apply.

**Best Practices:**
- Plan ASGs to minimize the number of security rules.
- Use service tags or ASGs instead of individual IP addresses for better management.

## Plan and implement user-defined routes (UDRs)
#### Overview of User-Defined Routes (UDRs)
UDRs are custom routes that override Azure's default system routes or add new routes to a subnet's route table. Each subnet can be associated with one route table.

**Key Points:**
- UDRs override default routes if there are conflicts.
- Each subnet can have zero or one route table.
- Route tables can be associated with multiple subnets.

#### Next Hop Types for UDRs
When creating UDRs, you can specify the following next hop types:

1. **Virtual Appliance:**
   - **Definition:** A VM running a network application like a firewall.
   - **Next Hop IP Address:** Can be the private IP of a NIC attached to a VM.
   - **Enable IP Forwarding:** Required for VMs forwarding traffic.
   - **Use Cases:** Inspecting and routing traffic; performing NAT for traffic heading to public IPs.

2. **Virtual Network Gateway:**
   - **Definition:** Directs traffic to a virtual network gateway, created with type VPN.
   - **Restrictions:** Cannot use with ExpressRoute gateways in UDRs.
   - **0.0.0.0/0 Prefix:** Can be used to route all traffic to the gateway.

3. **None:**
   - **Definition:** Drops traffic destined for the specified address prefix.
   - **Usage:** Used for system routes where devices aren't fully configured.

4. **Virtual Network:**
   - **Definition:** Overrides default routing within a virtual network.

5. **Internet:**
   - **Definition:** Routes traffic destined for specified address prefixes to the Internet.
   - **Usage:** Ensures traffic to Azure services with public IPs stays within the Azure backbone network.

**Note:** Virtual network peering and VirtualNetworkServiceEndpoint cannot be specified in UDRs and are created by Azure automatically.

#### Service Tags for UDRs
Service tags simplify route management by representing groups of IP prefixes for Azure services. Microsoft manages and updates these prefixes automatically.

**Benefits of Service Tags:**
- Reduces complexity and need for frequent updates.
- Limits the number of routes required.

**Route Evaluation Order:**
1. Regional tags (e.g., Storage.EastUS)
2. Top-level tags (e.g., Storage, AppService)
3. AzureCloud regional tags (e.g., AzureCloud.canadacentral)
4. AzureCloud tag

**Creating a Route with a Service Tag:**

**PowerShell:**
```powershell
$param = @{
  Name = 'StorageRoute'
  AddressPrefix = 'Storage'
  NextHopType = 'VirtualAppliance'
  NextHopIpAddress = '10.0.100.4'
}
New-AzRouteConfig @param
```

**Azure CLI:**
```sh
az network route-table route create \
  --resource-group MyResourceGroup \
  --route-table-name MyRouteTable \
  --name StorageRoute \
  --address-prefix Storage \
  --next-hop-type VirtualAppliance \
  --next-hop-ip-address 10.0.100.4
```

#### Best Practices and Considerations
- **Planning:** Plan route tables and UDRs carefully to avoid conflicts and ensure efficient routing.
- **IP Forwarding:** Ensure IP forwarding is enabled on VMs and NICs as needed.
- **Service Tags:** Use service tags to minimize maintenance and reduce the number of routes.
- **0.0.0.0/0 Routes:** Be cautious with routes that use the 0.0.0.0/0 prefix as they route all traffic.
## Plan and implement Virtual Network peering or VPN gateway

#### Key Concepts
- **Virtual Network:** An isolated segment of the Azure public network.

#### Virtual Network Connection Types
1. **Virtual Network Peering:**
   - **Purpose:** Connects two Azure virtual networks, making them appear as one.
   - **Routing:** Through Microsoft backbone, using private IPs only, no public internet.
   - **Benefits:** Low-latency, high-bandwidth connection, ideal for cross-region data replication and database failover.
   - **Usage:** Scenarios with strict data policies avoiding public internet.

2. **VPN Gateway:**
   - **Purpose:** Connects Azure virtual networks to on-premises locations or other Azure VNETs over the public internet.
   - **Bandwidth:** Limited; suitable for encryption-needed scenarios with bandwidth tolerance.
   - **Each VNET:** Can have only one VPN gateway.
   - **DDoS Protection:** Recommended on perimeter virtual networks.

#### Gateway Transit
- **Purpose:** Share an ExpressRoute or VPN gateway among peered VNETs.
- **Benefits:** Cost savings and reduced management overhead.
- **Usage:** Allows centralized connectivity management and simplifies network architecture.

#### Configuring Connections
- **Supported Connections:**
  - Virtual networks in different regions.
  - Different Microsoft Entra tenants.
  - Different Azure subscriptions.
  - Mix of Azure deployment models (Resource Manager and classic).

#### Comparison: Virtual Network Peering vs. VPN Gateway
| **Item**                    | **Virtual Network Peering**                                         | **VPN Gateway**                                                       |
| --------------------------- | ------------------------------------------------------------------- | --------------------------------------------------------------------- |
| **Limits**                  | Up to 500 peerings per VNET                                         | One VPN gateway per VNET, tunnels depend on SKU                       |
| **Pricing Model**           | Ingress/Egress                                                      | Hourly + Egress                                                       |
| **Encryption**              | Recommended software-level encryption                               | Custom IPsec/IKE policy                                               |
| **Bandwidth Limitations**   | None                                                                | Varies by SKU                                                         |
| **Private?**                | Yes, private via Microsoft backbone                                 | Public IP involved, private if Microsoft global network enabled       |
| **Transitive Relationship** | Non-transitive, requires NVAs or gateways for transitive networking | Transitive if BGP is enabled                                          |
| **Initial Setup Time**      | Fast                                                                | ~30 minutes                                                           |
| **Typical Scenarios**       | Data replication, database failover, frequent backups of large data | Encryption-specific, not latency-sensitive, no high throughput needed |

#### Exam Tips
- **Virtual Network Peering:** Best for low-latency and high-bandwidth internal Azure communication.
- **VPN Gateway:** Best for secure, encrypted connections, tolerable to bandwidth limitations.
- **Gateway Transit:** Use for efficient and centralized connectivity across multiple VNETs.
- **Scenarios:** Match the connection type with specific needs like low latency or high security.
## Plan and implement Virtual WAN, including secured virtual hub

#### Key Concepts
- **Virtual WAN:** Connects resources in Azure using an IPsec/IKE VPN connection.
- **Virtual Hub:** Central virtual network used by Virtual WAN.

#### Prerequisites
- **Azure Subscription:** Ensure you have an active Azure subscription.
- **IP Address Range:** Define a non-overlapping IP address range for the virtual hub.
- **Azure Tools:** Use Azure portal or Azure PowerShell for configuration.

#### Steps for Implementation

1. **Create a Resource Group:**
   ```powershell
   New-AzResourceGroup -Location "East US" -Name "TestRG"
   ```

2. **Create a Virtual WAN:**
   ```powershell
   $virtualWan = New-AzVirtualWan -ResourceGroupName TestRG -Name TestVWAN1 -Location "East US"
   ```

3. **Create a Virtual Hub:**
   ```powershell
   $virtualHub = New-AzVirtualHub -VirtualWan $virtualWan -ResourceGroupName "TestRG" -Name "Hub1" -AddressPrefix "10.1.0.0/16" -Location "westus"
   ```

4. **Create a Site-to-Site VPN Gateway:**
   ```powershell
   New-AzVpnGateway -ResourceGroupName "TestRG" -Name "vpngw1" -VirtualHubId $virtualHub.Id -VpnGatewayScaleUnit 2
   ```

5. **Create VPN Sites and Connections:**
   - **VPN Site Address Spaces:**
     ```powershell
     $vpnSiteAddressSpaces = New-Object string[] 2
     $vpnSiteAddressSpaces[0] = "192.168.2.0/24"
     $vpnSiteAddressSpaces[1] = "192.168.3.0/24"
     ```
   - **VPN Site Links:**
     ```powershell
     $vpnSiteLink1 = New-AzVpnSiteLink -Name "TestSite1Link1" -IpAddress "15.25.35.45" -LinkProviderName "SomeTelecomProvider" -LinkSpeedInMbps "10"
     $vpnSiteLink2 = New-AzVpnSiteLink -Name "TestSite1Link2" -IpAddress "15.25.35.55" -LinkProviderName "SomeTelecomProvider2" -LinkSpeedInMbps "100"
     ```
   - **Create VPN Site:**
     ```powershell
     $vpnSite = New-AzVpnSite -ResourceGroupName "TestRG" -Name "TestSite1" -Location "westus" -VirtualWan $virtualWan -AddressSpace $vpnSiteAddressSpaces -DeviceModel "SomeDevice" -DeviceVendor "SomeDeviceVendor" -VpnSiteLink @($vpnSiteLink1, $vpnSiteLink2)
     ```

6. **Connect the VPN Site to a Hub:**
   ```powershell
   New-AzVpnConnection -ResourceGroupName $vpnGateway.ResourceGroupName -ParentResourceName $vpnGateway.Name -Name "testConnection" -VpnSite $vpnSite -VpnSiteLinkConnection @($vpnSiteLinkConnection1, $vpnSiteLinkConnection2)
   ```

7. **Connect a VNet to the Hub:**
   - **Create a VNet:**
     ```powershell
     $vnet = @{ Name = 'VNet1' ResourceGroupName = 'TestRG' Location = 'eastus' AddressPrefix = '10.21.0.0/16' }
     $virtualNetwork = New-AzVirtualNetwork @vnet
     ```
   - **Specify Subnet Settings:**
     ```powershell
     $subnet = @{ Name = 'Subnet-1' VirtualNetwork = $virtualNetwork AddressPrefix = '10.21.0.0/24' }
     $subnetConfig = Add-AzVirtualNetworkSubnetConfig @subnet
     $virtualNetwork | Set-AzVirtualNetwork
     ```
   - **Connect VNet to Hub:**
     ```powershell
     New-AzVirtualHubVnetConnection -ResourceGroupName "TestRG" -VirtualHubName "Hub1" -Name "VNet1-connection" -RemoteVirtualNetwork $remoteVirtualNetwork
     ```

8. **Download and Configure VPN Device:**
   - **Download VPN Config:**
     From Virtual WAN page -> Hubs -> Your virtual hub -> VPN (Site to site) page -> Download VPN Config.
   - **Apply Configuration:**
     Use the configuration file to set up your on-premises VPN device.

#### Exam Tips
- **Virtual WAN:** Centralized network with site-to-site connectivity.
- **Virtual Hub:** Key element within Virtual WAN.
- **Azure Tools:** Familiarize with Azure portal and PowerShell for network configuration.
- **Steps:** Follow structured steps for resource creation, connection setup, and VPN configuration.

## Secure VPN connectivity, including point-to-site and site-to-site
- **VPN Gateway Configurations**: Determine the best fit for your needs.
- **Site-to-Site (S2S) VPN**: For cross-premises and hybrid setups, needs on-premises VPN device.
- **Point-to-Site (P2S) VPN**: Connects individual client computers securely, ideal for remote workers.
- **VNet-to-VNet Connections**: Secure tunnel between Azure VNets, supports various configurations.
- **Connections between Deployment Models**: Connect VNets across classic and Resource Manager models.
- **VNet Peering**: Alternative to VPN gateway for certain network setups.
- **S2S and ExpressRoute Coexisting Connections**: Combine secure VPN and ExpressRoute for enhanced connectivity. Requires dual gateways for the same VNet.

## ExpressRoute
- **Azure ExpressRoute Overview**: Extends on-premises networks to Microsoft cloud via private connection.
- **Connectivity Options**: Any-to-any (IP VPN), point-to-point Ethernet, or virtual cross-connection.
- **Benefits**: Enhanced reliability, speed, security, and latency compared to typical Internet connections.
- **Key Features**:
  - Layer 3 connectivity using BGP.
  - Redundancy with dual connections to Microsoft Enterprise Edge routers.
  - SLA for connection uptime.
  - Quality of Service (QoS) support for Skype for Business.
- **Connectivity to Microsoft Services**: Azure, Microsoft 365 across regions.
- **ExpressRoute Premium**: Enables global connectivity and dynamic scaling of bandwidth.
- **ExpressRoute Local**: Cost-effective data transfer near Azure regions.
- **ExpressRoute Global Reach**: Data exchange across on-premises sites.
- **ExpressRoute Direct**: Direct 100-Gbps connectivity into Microsoft’s global network.
- **Bandwidth Options**: Range from 50 Mbps to 10 Gbps with dynamic scaling.
- **Billing Models**: Unlimited data, metered data, ExpressRoute premium add-on for increased capabilities.
### **Implementing Encryption over ExpressRoute**: 
- **Purpose**: Establish encrypted transit between on-premises networks and Azure virtual networks via ExpressRoute without using public Internet or IP addresses.
- **Topology**: Connect on-premises network to Azure hub VPN gateway over ExpressRoute private peering.
- **Routing**: Azure prefixes (hub and spoke virtual networks) are advertised via ExpressRoute private peering BGP and VPN BGP for traffic from on-premises networks to Azure.
- **Traffic Direction**: Ensure Azure routes via VPN are preferred over direct ExpressRoute path for both inbound and outbound traffic.
- **Configuration Steps**:
  1. **Create Virtual WAN and Hub**: Includes ExpressRoute and VPN gateways.
  2. **Create Site for On-Premises Network**: Configure VPN site, IP addresses, and BGP settings.
  3. **Update VPN Connection Setting to Use ExpressRoute**: Configure VPN gateway to use private IP addresses over ExpressRoute.
  4. **Get Private IP Addresses for Hub VPN Gateway**: Download VPN device configuration file.
  5. **Configure VPN Device**: Apply settings from the configuration file to on-premises VPN device.
  6. **View Virtual WAN and Monitor Connections**: Monitor hub, site, and VPN connection status on the Virtual WAN dashboard.
## Configure firewall settings on PaaS resources
- **Purpose**: Secure applications on Azure by configuring firewall settings on Platform as a Service (PaaS) resources to protect against common exploits and vulnerabilities.
- **Cloud Security Advantages**:
  - Clear division of responsibility between you and Microsoft.
  - Cloud-based security capabilities and intelligence improve threat detection and response times.
- **Security Advantages of PaaS Model**:
  - Physical infrastructure risks mitigated by Microsoft.
  - Strong DDoS protection provided.
  - Shift in security approach from network perimeter to identity perimeter.
- **Best Practices for Managing Identity Perimeter**:
  - Secure keys and credentials using Azure Key Vault.
  - Avoid storing credentials in source code or public repositories.
  - Protect VM management interfaces with strong authentication and authorization platforms.
  - Adopt a policy of identity as the primary security perimeter.
- **Best Practices for Azure App Service**:
  - Authenticate through Microsoft Entra ID.
  - Restrict access based on least privilege security principles.
  - Safeguard keys using Azure Key Vault.
  - Restrict incoming source IP addresses.
  - Monitor security state using Microsoft Defender for Cloud.
- **Azure Cloud Services**:
  - Install a web application firewall (WAF) for centralized protection against common exploits and vulnerabilities.
  - Enable Azure DDoS Protection and monitor application performance using Azure Application Insights.
  - Conduct regular security penetration testing, including fuzz testing, to validate security defenses.
## Monitor network security by using Network Watcher, including NSG flow logging
- **Purpose**: Utilize Azure Network Watcher to monitor, diagnose, and manage network security for Azure IaaS (Infrastructure-as-a-Service) resources.
- **Features of Network Watcher**:
  - **Monitoring**:
    - **Topology**: Visualize network configuration across subscriptions, resource groups, and locations.
    - **Connection Monitor**: Monitor end-to-end connection performance between various endpoints in your network infrastructure.
  - **Network Diagnostic Tools**:
    - **IP Flow Verify**: Detect traffic filtering issues at the virtual machine level and identify security rules affecting traffic.
    - **NSG (Network Security Group) Diagnostics**: Detect traffic filtering issues at the VM, VM scale set, or application gateway level. Add new security rules for traffic management.
    - **Next Hop**: Identify routing issues and ensure traffic is routed correctly to intended destinations.
    - **Effective Security Rules**: View the effective security rules applied to a network interface or subnet.
    - **Connection Troubleshoot**: Test connections between different resources and troubleshoot connectivity issues.
    - **Packet Capture**: Remotely capture packet data to track traffic to and from VMs or VM scale sets.
    - **VPN Troubleshoot**: Troubleshoot virtual network gateways and their connections.
  - **Traffic Tools**:
    - **Flow Logs**: Log Azure IP traffic information and store data in Azure storage for analysis.
    - **Traffic Analytics**: Visualize flow logs data to gain insights into network traffic patterns.
- **Automated Activation**: Network Watcher is automatically enabled in the region of your virtual network without impacting resources or incurring additional charges.
- **Use Cases**:
  - Diagnose network issues such as traffic filtering, routing problems, and connectivity errors.
  - Visualize network topology to understand resource relationships and configurations.
  - Monitor end-to-end connection performance for Azure and hybrid endpoints.
  - Analyze traffic patterns and security events using flow logs and traffic analytics.
- **Benefits**:
  - Enhanced visibility into network health and performance.
  - Streamlined troubleshooting and diagnostics for network-related issues.
  - Improved security posture through effective management of security rules and traffic monitoring.
- **Limitations**: Network Watcher is not designed for PaaS monitoring or web analytics, focusing solely on IaaS resources.

# Plan and implement security for private access to Azure resources

## Plan and implement virtual network Service Endpoints
- **Purpose**: Utilize virtual network service endpoints to provide secure and direct connectivity to Azure services over the Azure backbone network.
- **Features of Service Endpoints**:
  - **Secure Connectivity**: Establish secure connectivity between virtual networks and Azure services without the need for public IP addresses on the VNet.
  - **Optimized Routing**: Route Azure service traffic directly from the virtual network to the service over the Azure backbone network, ensuring optimal routing.
  - **Simplified Setup**: Configure service endpoints on subnets without the need for reserved public IP addresses, NAT, or gateway devices.
- **Benefits**:
  - **Improved Security**: Secure Azure service resources to virtual networks, removing public internet access and allowing traffic only from the VNet.
  - **Optimal Routing**: Direct service traffic over the Azure backbone network, ensuring efficient data transfer and monitoring capabilities.
  - **Reduced Management Overhead**: Simplified setup process with no additional infrastructure required for securing Azure resources to virtual networks.
- **Limitations**:
  - Service endpoints are available only for virtual networks deployed through the Azure Resource Manager deployment model.
  - Endpoints are enabled on subnets and cannot be used for traffic from on-premises services to Azure services.
  - Azure SQL Database service endpoints are limited to traffic within a virtual network's region.
- **Configuration**:
  - Configure service endpoints on subnets within virtual networks to enable connectivity to supported Azure services.
  - Multiple service endpoints can be configured on a subnet, and they work with any type of compute instances within that subnet.
  - Virtual networks and Azure service resources can be in the same or different subscriptions.
- **Considerations**:
  - Enabling a service endpoint switches source IP addresses from public IPv4 addresses to private IPv4 addresses for service traffic from the virtual network.
  - Ensure existing Azure service firewall rules allow for this switch before setting up service endpoints to avoid interruptions in service traffic.
  - DNS entries for Azure services remain unchanged and continue to resolve to public IP addresses.
- **Scenarios**:
  - Secure Azure services to multiple subnets within a virtual network or across multiple virtual networks by enabling service endpoints on each subnet independently.
  - Filter outbound traffic from a virtual network to Azure services by deploying a network virtual appliance and applying service endpoints to the subnet.
  - Secure Azure service resources to managed service subnets by setting up service endpoints on the managed service subnet directly.
- **Logging and Troubleshooting**:
  - Validate service endpoint routes by checking the source IP address of service requests and viewing effective routes on network interfaces in subnets.
  - Service endpoint routes override any BGP routes for Azure service traffic.
- **Provisioning**:
  - Users with write access to virtual networks can configure service endpoints independently.
  - Permission to join subnets via service endpoint action is required to secure Azure service resources to a VNet.
  - Virtual networks and Azure service resources can be in the same or different subscriptions, and certain Azure services support service endpoints across different tenants.
- **VNet Service Endpoint Policies**:
  - Filter virtual network traffic to Azure services using service endpoint policies, allowing only specific Azure service resources over service endpoints.
  - Service endpoint policies provide granular access control for virtual network traffic to Azure services.
- **Pricing and Limits**:
  - No additional charge for using service endpoints; existing Azure service pricing applies.
  - No limit on the total number of service endpoints in a virtual network.
  - Certain Azure services may enforce limits on the number of subnets used for securing the resource.
## Plan and implement Private Endpoints
- **Purpose**: Utilize private endpoints to establish private and secure connections between a virtual network and services powered by Azure Private Link.
- **Properties of Private Endpoints**:
  - **Name**: A unique identifier within the resource group.
  - **Subnet**: The subnet where the private IP address is assigned.
  - **Private-link resource**: The resource to connect, identified by a resource ID or alias.
  - **Target subresource**: The specific subresource to connect within the private-link resource.
  - **Connection approval method**: Automatic or manual approval for establishing the connection.
  - **Request message**: Optional message for manually requested connections.
  - **Connection status**: Indicates whether the private endpoint is active, approved, pending, rejected, or disconnected.
- **Considerations**:
  - Private endpoints facilitate connectivity between customers within the same virtual network, regionally or globally peered virtual networks, on-premises environments via VPN or Express Route, and services powered by Private Link.
  - Connections can only be initiated by clients connecting to the private endpoint; service providers do not have routing configurations to establish connections into service customers.
  - A read-only network interface is automatically created for the private endpoint, assigned a dynamic private IP address from the subnet mapping to the private-link resource.
  - Private endpoints must be deployed in the same region and subscription as the virtual network, while the private-link resource can be deployed in a different region.
- **Network Security of Private Endpoints**:
  - Traffic is secured to a private-link resource, with the platform validating network connections and allowing only those reaching the specified resource.
  - Private endpoints support network policies such as Network Security Groups (NSG), User Defined Routes (UDR), and Application Security Groups (ASG).
- **Access to Private-link Resource using Approval Workflow**:
  - Connection approval methods include automatic approval (if permissions allow) or manual request initiation, requiring approval from the private-link resource owner.
  - The private-link resource owner can review, approve, reject, or delete private-endpoint connections as needed.
- **Connect using an Alias**:
  - An alias is a unique moniker generated for a private-link service behind a standard load balancer, allowing consumers to request a connection using the alias.
  - Manual approval for connection requests using an alias may be auto-approved if the consumer's subscription is allow-listed on the provider side.
- **DNS Configuration**:
  - DNS settings are crucial for connecting to a private-link resource; ensure correct DNS settings to resolve the fully qualified domain name (FQDN) to the private IP address of the private endpoint.
  - The network interface associated with the private endpoint contains necessary information for DNS configuration, including FQDN and private IP address.
- **Limitations**:
  - Static IP address configuration is currently unsupported for certain services like Azure Kubernetes Service (AKS), Azure Application Gateway, HDInsight, etc.
  - Network security group (NSG) limitations include unavailable effective routes and security rules for private endpoint network interfaces, unsupported NSG flow logs for inbound traffic to private endpoints, etc.
  - Application security group (ASG) limitations include unavailability in select regions.
- **Additional Considerations**:
  - SNAT is recommended for traffic destined to a private endpoint to ensure return traffic is honored due to the variable nature of the private endpoint data-plane.
  - Certain services may require all destination ports to be open when using a private endpoint and adding NSG security filters, such as Azure Cosmos DB.
- **Feature Availability**:
  - Private endpoints may be currently unavailable in select regions like West India, Australia Central 2, South Africa West, Brazil Southeast, all Government regions, and all China regions.
## Plan and implement Private Link services
- **Purpose**: Enable private access to your own service running behind Azure Standard Load Balancer using Azure Private Link.
- **Workflow**:
  - **Create Your Private Link Service**:
    - Configure your application behind a standard load balancer in your virtual network.
    - Create a Private Link Service referencing the load balancer, selecting frontend IP configuration and subnet for NAT IP addresses.
  - **Share Your Service**:
    - Azure generates a globally unique alias for your service, which you can share with customers for initiating Private Link connections.
  - **Manage Connection Requests**:
    - Service provider can accept or reject connection requests listed under the privateendpointconnections property on the Private Link service.
  - **Delete Your Service**:
    - If the service is no longer in use, delete it after ensuring no associated private endpoint connections.
- **Properties**:
  - **Provisioning State**: Indicates the current provisioning state of the Private Link service.
  - **Alias**: A globally unique string generated by Azure for the service, facilitating easy sharing.
  - **Visibility**: Controls the exposure settings for the service, allowing restriction based on Azure role-based access control permissions or specific subscriptions.
  - **Auto Approval**: Automates access to the Private Link service, approving connections from specified subscriptions.
  - **Load Balancer Frontend IP Configuration**: Tied to the frontend IP address of a Standard Load Balancer, directing traffic to backend pools.
  - **NAT IP Configuration**: NAT IP for the Private Link service, chosen from any subnet in the service provider's virtual network.
  - **Private Endpoint Connections**: Lists private endpoints connecting to the Private Link service, with individual control over their state.
  - **TCP Proxy V2 (EnableProxyProtocol)**: Enables retrieving connection information from private endpoint using TCP proxy v2.
- **Details**:
  - Private Link service can be accessed from approved private endpoints in any public region, including from globally peered virtual networks and on-premises via private VPN or ExpressRoute connections.
  - Upon creation, a network interface is created for the Private Link service's lifecycle, managed by Azure.
  - Must be deployed in the same region as the virtual network and Standard Load Balancer.
  - A single Private Link service can be accessed from multiple Private Endpoints belonging to different virtual networks, subscriptions, or Active Directory tenants.
  - Multiple Private Link services can be created on the same Standard Load Balancer using different frontend IP configurations.
  - More than one NAT IP configuration can be linked to a service, facilitating scalability.
- **Alias**:
  - Composed of Prefix.GUID.Suffix, where Prefix is the service name, GUID ensures global uniqueness, and Suffix indicates the Azure region.
  - Helps mask customer data and creates an easy-to-share name for the service.
- **Control Service Exposure**:
  - Visibility settings control whether consumers can connect to the service, offering options from most restrictive to least restrictive.
- **Control Service Access**:
  - Consumers can create private endpoints and request connections to the Private Link service, with service providers approving or rejecting these requests.
  - Auto-approval automates the approval process for specified subscriptions.
- **Getting Connection Information using TCP Proxy v2**:
  - Enables retrieving source IP address and LinkID from private endpoints using TCP proxy v2, facilitating unique consumer identification.
  - Custom Type-Length-Value (TLV) vector encodes this information.
- **Limitations**:
  - Supported only on Standard Load Balancer, with specific backend pool configurations and IPv4 traffic.
  - Supports TCP and UDP traffic only.
  - Private Link Service has an idle timeout of ~5 minutes and requires TCP Keepalives lower than that to avoid hitting this limit.
## Plan and implement network integration for Azure App Service and Azure Functions
- **Purpose**: Enable Azure App Service to securely access resources within a virtual network. Also Azure App Service is a fully managed platform for building, deploying, and scaling web and mobile applications.
- **Variations**: Available for dedicated compute pricing tiers and App Service Environments.
- **Key Features**: Outbound calls from apps to virtual network resources, supported in various pricing tiers, TCP and UDP support.
- **Limitations**: Excludes drive mounting, Windows AD domain join, and NetBIOS. Region dependency, no IPv6 support in integration virtual networks.
- **How It Works**: Mounts virtual interfaces to worker roles, outbound calls through virtual network, outbound addresses from integration subnet.
- **Subnet Requirements**: Requires unused subnet of at least IPv4 /28 block, one address consumed per App Service plan instance.
- **Permissions**: RBAC permissions needed for subnet configuration, actions include reading virtual network definitions and joining subnets.
- **Routing Options**: Application, configuration, and network routing control traffic flow.
- **Service Connectivity**: Access Azure services with service endpoints, private endpoints require DNS lookup resolution.
- **Peering and On-Premises Connectivity**: No extra config needed for peering, connect to on-premises resources via ExpressRoute or site-to-site VPN.
- **Management and Troubleshooting**: Managed at app level, no extra charge beyond App Service plan pricing tier, troubleshoot using troubleshooting guide.
- **Best Practices**: Use adequate subnet size, configure routing carefully, ensure proper RBAC setup.
## Plan and implement network security configurations for an App Service Environment (ASE)
- **Purpose**: Securely deploy Azure App Service within a customer's Azure Virtual Network.
- **Components**: Front ends (HTTP/HTTPS termination), workers (hosting apps), database, storage.
- **Deployment Options**: External ASE (external VIP), ILB ASE (internal VIP with ILB).
- **Creation Process**: Similar to regular app creation but select ASE as the location and Isolated pricing tier.
- **Scaling**: Scale App Service plans and front ends simultaneously. ASE can scale up to 100 instances and 201 total instances across all plans.
- **IP Addressing**: Dedicated IP allocation, IP-based TLS/SSL binding, limited IP addresses with External ASE, internal DNS management for ILB ASE.
- **Front-end Scaling**: Front ends automatically scale based on App Service plan instances, adjustable ratio and size with additional charges.
- **App Access**: Domain suffix varies based on ASE type, SCM URL for deployment, DNS configuration needed for ILB ASE.
- **Publishing**: Multiple methods including web deployment, FTP, CI, IDEs, but ILB ASE endpoints only accessible internally.
- **Storage**: 1 TB storage for all apps in ASE, 250 GB per App Service plan.
- **Monitoring**: Monitor App Service plans, individual apps, and platform infrastructure metrics.
- **Logging**: Integrate ASE with Azure Monitor for logging platform events, configure alerts based on log triggers.
- **Upgrade Preference**: Option to prioritize ASE upgrades, choose between None, Early, or Late upgrade batches.
- **Pricing**: Isolated pricing SKU for ASE, flat rate for ASE infrastructure plus charges for additional front-end scaling.
- **Deletion**: Delete ASE and all its content, confirmation required.
- **CLI**: Azure CLI commands available for ASE administration.
## Plan and implement network security configurations for an Azure SQL Managed Instance
- **Security Baseline**: Microsoft cloud security benchmark version 1.0 applied to Azure SQL.
- **Monitoring**: Utilize Microsoft Defender for Cloud to monitor security baseline and Azure Policy definitions.
- **Security Profile**:
  - Product Category: Databases
  - Customer Access to HOST / OS: No Access
  - Deployment into Customer's Virtual Network: True
  - Stores Customer Content at Rest: True
- **Network Security**:
  - **NS-1: Establish Network Segmentation Boundaries**:
    - Virtual Network Integration: Deploy service into customer's private VNet, assign private IPs unless strong reason for public IPs.
    - Network Security Group Support: Utilize Azure Virtual Network Service Tags for NSG rules.
  - **NS-2: Secure Cloud Services with Network Controls**:
    - Azure Private Link: Deploy private endpoints for all Azure resources supporting Private Link feature.
    - Disable Public Network Access: Supported and enabled by default, can be configured using IP ACL or toggle switch.
  - **Microsoft Defender for Cloud Monitoring**:
    - Azure Policy built-in definitions for Azure SQL Managed Instances and Azure SQL Database.
    - Recommendations include disabling public network access, enabling private endpoint connections, and enforcing access only from private endpoints.

# Plan and implement security for public access to Azure resources

## Plan and implement Transport Layer Security (TLS) to applications, including Azure App Service and API Management
- **Encryption Overview**:
  - Encryption in Microsoft Azure encompasses encryption at rest, encryption in transit, and key management with Azure Key Vault.
- **Data Encryption at Rest**:
  - Azure provides encryption for various data storage solutions including file, disk, blob, and table storage.
  - Encryption models include server-side encryption with service-managed keys, customer-managed keys in Key Vault, or client-side encryption.
- **Azure Encryption Models**:
  - **Client-side Encryption**: Encryption performed outside Azure, maintaining complete control of keys.
  - **Server-side Encryption**:
    - Service-managed keys: Balances control and convenience with low overhead.
    - Customer-managed keys: Offers control over keys, including BYOK support.
    - Service-managed keys in customer-controlled hardware (HYOK): Allows managing keys in proprietary repositories.
- **Azure Disk Encryption**: Protects managed disks for Linux and Windows VMs using DM-Crypt or BitLocker.
- **Azure Storage Service Encryption (SSE)**:
  - Encrypts data at rest in Azure Blob storage and file shares, transparently to users, using AES encryption.
  - Supports both server-side and client-side encryption scenarios.
- **Azure SQL Database Encryption**:
  - **Transparent Data Encryption (TDE)**: Encrypts data files in real time using DEK, stored in the database boot record.
  - **Always Encrypted**: Encrypts data within client applications prior to storing it in Azure SQL Database, maintaining separation between data owners and managers.
  - **Cell-level Encryption (CLE)**: Allows applying symmetric encryption to specific columns or cells of data using T-SQL.
- **Azure Cosmos DB Encryption**: Encrypts user data stored in non-volatile storage by default, with optional customer-managed keys.
- **Encryption in Data Lake**: Supports encryption of data at rest and in transit, with options to manage keys yourself.
- **Data Encryption in Transit**:
  - **Data-link Layer Encryption in Azure**: Uses MACsec encryption standards by default for Azure traffic.
  - **TLS Encryption in Azure**: Protects data traveling between cloud services and customers, ensuring strong authentication, message privacy, and integrity.
- **Azure Storage Transactions**: All interactions with Azure Storage occur over HTTPS, enforcing secure transfer.
- **Server Message Block (SMB) Encryption**: Encrypts data in transit over Azure Virtual Networks using SMB 3.0.
- **In-transit Encryption in VMs**: Data to, from, and between VMs running Windows can be encrypted using various protocols including RDP and SSH.
- **Azure VPN Gateways**: Enable encrypted traffic between virtual networks and on-premises locations using IPsec/IKEv1 or IKEv2 VPN tunnels.
- **Point-to-site VPNs**: Allow individual client computers access to Azure virtual networks using SSTP protocol, traversing firewalls.
- **Site-to-site VPNs**: Connect on-premises networks to Azure virtual networks over IPSec/IKEv1 or IKEv2 VPN tunnels.
- **Key Management with Key Vault**: Manages and controls access to encryption keys used by cloud services, ensuring proper protection and management of keys without direct access by applications.
## Plan, implement, and manage an Azure Firewall, including Azure Firewall Manager and firewall policies
- **Azure Firewall Overview**:
  - Azure Firewall is a cloud-native network firewall service providing stateful firewall as a service for Azure workloads.
  - It offers both east-west and north-south traffic inspection.
- **SKU Offerings**:
  - **Standard**: Provides L3-L7 filtering and threat intelligence feeds for real-time protection.
  - **Premium**: Offers signature-based intrusion detection and prevention system (IDPS) with over 67,000 signatures across 50 categories.
  - **Basic**: Intended for SMB customers with essential protection features and fixed scale unit.
- **Azure Firewall Manager**:
  - Central security management service for Azure Firewall policies and route management.
  - Manages security for secured virtual hubs (Azure Virtual WAN Hub) and hub virtual networks.
- **Features of Azure Firewall Manager**:
  - **Central Deployment and Configuration**: Centrally deploy and configure Azure Firewall instances across different regions and subscriptions.
  - **Hierarchical Policies**: Author global firewall policies for organization-wide enforcement and locally authored policies for DevOps agility.
  - **Integration with Third-party Security-as-a-Service (SECaaS)**: Integrate third-party security services for additional network protection.
  - **VNet to Internet (V2I) Traffic Filtering**: Filter outbound virtual network traffic with third-party security providers for advanced user-aware Internet protection.
  - **Branch to Internet (B2I) Traffic Filtering**: Easily add third-party filtering for branch to Internet scenarios.
  - **Centralized Route Management**: Route traffic to secured hub for filtering and logging without manual setup of User Defined Routes (UDR).
  - **DDoS Protection Plan**: Associate virtual networks with DDoS protection plan within Azure Firewall Manager.
  - **Manage Web Application Firewall (WAF) Policies**: Centrally create and associate WAF policies for application delivery platforms.
  - **Region Availability**: Azure Firewall Policies can be used across regions for flexibility in policy creation and enforcement.
## Plan and implement an Azure Application Gateway
- **Azure Application Gateway Overview**:
  - Azure Application Gateway is a layer 7 web traffic load balancer designed to manage traffic to web applications.
  - Unlike traditional load balancers operating at OSI layer 4 (TCP and UDP), Application Gateway functions at layer 7 and can make routing decisions based on attributes like URI path or host headers.

- **Components**:
  - **Frontend IP Address**: Configurable with public, private, or dual-stack IP addresses to handle incoming requests.
  - **Listeners**: Logical entities checking for incoming connection requests based on port, protocol, host, and IP address.
  - **Request Routing Rules**: Determine how incoming requests are routed to backend pools based on various conditions.
  - **Backend Pools**: Collections of backend targets such as Azure VMs, virtual machine scale sets, Azure App Service, or on-premises/external servers.
  - **Backend HTTP Settings**: Configuration specifying how traffic is routed from Application Gateway to backend servers.
  - **Rewrite Rules**: Modify HTTP(S) request and response headers, URL path, and query string parameters.
  - **Cookie-based Affinity**: Maintains user sessions by routing subsequent requests carrying an affinity cookie to the same backend server.
  - **Connection Draining**: Gracefully removes backend pool members during planned service updates to avoid new requests while maintaining existing connections.
  - **Protocol and Port Configuration**: Specifies backend server communication protocol (HTTP/HTTPS) and port range (1-65535).
  - **Trusted Root Certificate**: Required for HTTPS backend protocol to establish trust between Application Gateway and backend servers.
  - **Request Timeout**: Specifies the duration for Application Gateway to wait for a response from backend servers.

- **Features and Functionality**:
  - **Layer 7 Load Balancing**: Allows routing decisions based on additional attributes of an HTTP request, enabling URL-based routing and more.
  - **Flexible Frontend Configuration**: Supports public, private, or dual-stack IP addresses, allowing for varied deployment scenarios.
  - **Path-based Routing**: Routes requests from specific URL paths to corresponding backend pools, enhancing granularity in traffic management.
  - **Listeners**: Essential for handling incoming connection requests, supporting multiple ports and protocols (HTTP, HTTPS, HTTP/2, WebSocket).
  - **Request Routing Rules**: Basic or path-based rules determine how requests are forwarded to backend pools based on specified conditions.
  - **Connection Draining**: Ensures backend pool members are gracefully removed during updates, maintaining existing connections until timeout.
  - **End-to-End TLS**: Supports HTTPS backend protocol for secure communication between Application Gateway and backend servers.
  - **Cookie-based Affinity**: Maintains session stickiness by routing subsequent requests from the same user session to the same backend server.
  - **Rewrite Rules**: Enables modification of HTTP(S) request and response headers, URL paths, and query string parameters for enhanced security and functionality.
  - **Centralized Management**: Provides a single point of contact for managing traffic to web applications, facilitating efficient traffic distribution and optimization.

- **Use Cases**:
  - **Web Application Load Balancing**: Ideal for distributing incoming web traffic across multiple backend servers based on various attributes.
  - **Session Persistence**: Ensures continuity of user sessions by directing subsequent requests from the same user session to the same backend server.
  - **Granular Traffic Management**: Allows for path-based routing, enabling routing decisions based on specific URL paths to different backend pools.
  - **Secure Communication**: Supports end-to-end TLS for encrypted communication between Application Gateway and backend servers, ensuring data security and integrity.
  - **Application Delivery**: Facilitates efficient delivery of web applications by intelligently routing traffic based on URI paths, host headers, or other attributes of HTTP requests.
- **Advanced Configuration**:
  - **Override Backend Path**: Allows configuring a custom forwarding path to use when requests are forwarded to the backend. Any part of the incoming path that matches the custom path in the override backend path field is copied to the forwarded path.
    - When attached to a basic request-routing rule, the original request is modified based on the override backend path.
    - Similarly, when attached to a path-based request-routing rule, the override backend path determines how the original request is forwarded to the backend.
  - **Use Custom Probe**: Associates a custom probe with an HTTP setting, providing greater control over backend health monitoring.
    - Only one custom probe can be associated with an HTTP setting, and if none is explicitly associated, the default probe is used.
    - Custom probes offer more flexibility and control in monitoring backend health compared to default probes.
- **Configuring Host Name**:
  - Application Gateway allows for the connection established to the backend to use a different hostname than the one used by the client to connect to Application Gateway.
  - While this configuration can be useful, overriding the hostname between the client, Application Gateway, and backend target should be done with care to avoid potential issues with absolute URLs, redirect URLs, and host-bound cookies.
  - Two aspects influence the Host HTTP header used by Application Gateway to connect to the backend:
    - **Pick Host Name from Backend Address**: Dynamically sets the host header in the request to the host name of the backend pool, useful when the domain name of the backend differs from the DNS name of the application gateway.
    - **Host Name Override**: Replaces the host header in the incoming request on the Application Gateway with the specified host name, enabling customization of the hostname used in the forwarded request to the backend server.
- **Backend Pool and Health Probes**:
  - **Backend Pool**: Can point to four types of backend members: specific virtual machine, virtual machine scale set, IP address/FQDN, or an app service.
  - After creating a backend pool, it must be associated with one or more request-routing rules, and health probes must be configured for each backend pool on the application gateway.
  - **Health Probes**: Azure Application Gateway monitors the health of all servers in its backend pool and automatically stops sending traffic to any server it considers unhealthy.
    - Probes continue to monitor unhealthy servers, and the gateway starts routing traffic to them again once probes detect them as healthy.
    - Default probe is automatically configured if no custom probe is set up, making an HTTP GET request to configured backend pool IP addresses or FQDNs.
    - Custom probes offer more granular control over health monitoring and can be configured as needed for specific backend pool requirements.
    - Backend health reports are updated based on probe refresh intervals and independent of user requests.
- **Default Health Probe Settings**:
  - **Probe Property** | **Value** | **Description**
  - Probe URL | \<protocol>://127.0.0.1:\<port>/ | Protocol and port inherited from associated backend HTTP settings.
  - Interval | 30 | Time in seconds before the next health probe is sent.
  - Time-out | 30 | Time in seconds the application gateway waits for a probe response before marking it as unhealthy.
  - Unhealthy Threshold | 3 | Number of probes to send in case of failure before marking backend as unhealthy.

  The default probe checks only \<protocol>://127.0.0.1:\<port> for health status. Custom probes are required for configuring different URLs or modifying other settings.

- **Custom Health Probe**:
  - Allows granular control over health monitoring with custom hostname, URL path, probe interval, failed response threshold, etc.

  - **Custom Health Probe Settings**:
    - **Probe Property** | **Description**
    - Name | Identifies the probe in backend HTTP settings.
    - Protocol | Matches the protocol in associated backend HTTP settings.
    - Host | Used for the host header of the probe request (v1 SKU) or as host header and SNI (v2 SKU).
    - Path | Relative path of the probe.
    - Port | Destination port; defaults to HTTP settings port (v2 SKU).
    - Interval | Time interval between consecutive probes.
    - Time-out | Time to wait for a response before marking as failed.
    - Unhealthy Threshold | Number of consecutive probe failures before marking backend as unhealthy.

  - **Probe Matching**:
    - Supports HTTP response status code match and body match criteria for custom interpretation of healthy response.

  - **Custom Probe Use Cases**:
    - Configure probe traffic to accept 403 as an expected response for authenticated backend servers.
    - Use custom probe with specific hostname for successful TLS probe with wildcard certificates.

- **Network Security Group (NSG) Considerations**:
  - Provides fine-grain control over Application Gateway subnet via NSG rules.
  - Restrictions include allowing incoming Internet traffic on specific TCP ports and allowing outbound Internet connectivity.

- **How Application Gateway Works**:
  - Diagram depicts how Application Gateway accepts and routes requests.
  - DNS resolution resolves domain name to Application Gateway's frontend IP address.
  - Listener checks for connection requests and forwards valid requests to backend after WAF evaluation.

- **How Application Gateway Routes a Request**:
  - Based on request routing rule, selects backend pool and routes request to healthy backend server.
  - Uses round-robin algorithm for load balancing among healthy servers in backend pool.
  - Opens new TCP session with backend server based on HTTP settings.

- **Modifications to the Request**:
  - Application Gateway inserts additional headers to requests before forwarding to backend.
  - Headers include x-forwarded-for, x-forwarded-port, x-forwarded-proto, x-original-host, x-original-url, x-appgw-trace-id.
  - Headers facilitate backend processing and enable correlation between requests and diagnostic logs.
## Plan and implement an Azure Front Door, including Content Delivery Network (CDN)

- **Plan and Implement an Azure Front Door, including Content Delivery Network (CDN)**:
  - Azure Front Door is a modern cloud Content Delivery Network (CDN) by Microsoft, offering fast, reliable, and secure access to static and dynamic web content across the globe.

- **Key Benefits**:
  - **Global Delivery Scale**:
    - Utilizes Microsoft's global Cloud CDN and WAN, with over 118 edge locations across 100 metro cities.
    - Improves latency by up to 3 times and accelerates application performance using Front Door's anycast network and split TCP connections.
  - **Modern Apps and Architectures**:
    - Facilitates modernization of internet-first applications on Azure with Cloud Native experiences.
    - Supports integration with DevOps friendly command line tools and enables load balancing, routing, and health probe monitoring across Azure or external origins.
  - **Simple and Cost-effective**:
    - Offers unified static and dynamic delivery in a single tier, providing caching, SSL offload, and layer 3-4 DDoS protection.
    - Provides free, autorotation managed SSL certificates and a simplified cost model.
  - **Intelligent Secure Internet Perimeter**:
    - Ensures security with built-in layer 3-4 DDoS protection, Web Application Firewall (WAF), and Azure DNS.
    - Protects against layer 7 DDoS attacks using WAF and defends against malicious actors with Bot manager rules.
    - Facilitates private connection to backends behind Azure Front Door with Private Link.

- **Service Comparison**:
  - Azure Front Door and Azure CDN offer global content delivery with intelligent routing and caching capabilities at the application layer.
  - Both services secure applications from malicious attacks and provide monitoring capabilities.
  - Azure Front Door provides more advanced features such as enhanced rules engine, regular expressions, server variables, and integrated security options like Bot manager rules and Private Link connections.

- **Comparison Table**:
  - Provides a detailed comparison between Azure Front Door and Azure CDN services, covering features, optimizations, domains, certificates, caching, routing, security, analytics, ease of use, and pricing.

- **Pricing**:
  - Simplified pricing model with different tiers offering various features and optimizations.
  - Azure Front Door offers a low entry fee and reduced billing complexity.
## Plan and implement a Web Application Firewall (WAF)
- **Plan and Implement a Web Application Firewall (WAF)**:
  - WAF provides centralized protection against common exploits and vulnerabilities targeting web applications like SQL injection and cross-site scripting.

- **Supported Service**:
  - WAF can be deployed with Azure Application Gateway, Azure Front Door, and Azure Content Delivery Network (CDN).
  - WAF on Azure CDN is under public preview.

- **Azure Web Application Firewall (WAF) on Azure Application Gateway**:
  - Actively safeguards web applications against common exploits and vulnerabilities.
  - Based on the Core Rule Set (CRS) from OWASP.
  - WAF features are configured within a WAF policy, associable with Application Gateway, listeners, or path-based routing rules.

- **Application Gateway Functionality**:
  - Acts as an application delivery controller (ADC) providing TLS termination, session affinity, load distribution, content-based routing, and security enhancements.
  - Enhances security through TLS policy management and WAF integration, offering centralized protection and easy configuration.

- **Protection**:
  - Guards against web vulnerabilities without backend code modification.
  - Supports multiple protected websites per Application Gateway.
  - Allows creation of custom WAF policies for different sites.

- **Monitoring**:
  - Tracks attacks through real-time WAF logs integrated with Azure Monitor.

- **Customization**:
  - Customizes WAF rules and rule groups to eliminate false positives.
  - Supports site-specific WAF policies and custom rule creation.

- **Features**:
  - Provides protection against SQL injection, cross-site scripting, and other common web attacks.
  - Offers configurable request size limits and exclusion lists.
  - Allows creation of custom rules and geo-filtering.
  - Incorporates bot protection rule sets to block, allow, or log different types of bots.

- **WAF Policy and Rules**:
  - WAF policy includes managed rules and custom rules.
  - Custom rules have higher priority and are evaluated before managed rules.
  - Managed rule sets include Core Rule Sets, custom rules, and bot protection rule sets.

- **WAF Modes**:
  - Detection mode monitors and logs all threats without blocking requests.
  - Prevention mode blocks intrusions and attacks detected by rules.

- **WAF Engines**:
  - Runs on different engines depending on the CRS version.
  - Newer engines offer improved performance and features.

- **WAF Actions**:
  - Supported actions include Allow, Block, Log, and Anomaly Score.
  - Anomaly Scoring mode determines blocking based on severity of rule matches.

- **Configuration**:
  - WAF policies can be configured and deployed via Azure portal, REST APIs, ARM templates, and Azure PowerShell.

- **WAF Monitoring**:
  - Integrates with Azure Monitor for tracking diagnostic information and WAF alerts.
## Recommend when to use Azure DDoS Protection Standard
  - Azure DDoS Protection Standard is recommended when:
    - Deploying applications in a virtual network.
    - Needing protection at layer 3 and layer 4 network layers.
    - Requiring always-on monitoring, adaptive tuning, and detailed attack analytics.
    - Seeking integration with Azure services and turnkey protection.
    - Utilizing multi-layered protection with a Web Application Firewall (WAF).
    - Needing extensive mitigation scale and cost guarantee.

- **Best Practices**:
  - Maximize Azure DDoS Protection effectiveness by:
    - Designing applications with redundancy and resilience.
    - Implementing a multi-layered security approach.
    - Preparing an incident response plan.