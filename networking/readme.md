# Azure Networking - AZ-104 Cheatsheet

This section covers the networking components and concepts you need to understand for the AZ-104 Microsoft Azure Administrator exam.

## Key Networking Services

### Virtual Networks (VNets)
- **Region-specific**: VNets are regional resources and cannot span regions
- **Address space**: Uses CIDR notation (e.g., 10.0.0.0/16)
- **Limits**: Up to 5,000 VNets per subscription per region
- **Peering**: Connect VNets within same or different regions
- **Types of peering**: Regional (same region) and Global (cross-region)
- **Transitive routing**: Not supported by default in VNet peering

### Subnets
- Must be contained within the VNet address space
- Azure reserves 5 IP addresses in each subnet (first 4 and last 1)
- Minimum subnet size is /29 (8 IP addresses, 3 usable)
- Recommended minimum is /28 (16 IP addresses, 11 usable)
- Special subnet: AzureBastionSubnet must be named exactly this and be at least /27

### Network Security Groups (NSGs)
- **Region-bound**: NSGs can only be used in the same region they're created in
- **Resource Group flexibility**: Can be used across multiple resource groups within the same region
- **Association**: Can be associated with subnets and/or network interfaces
- **Rule processing**: Evaluated by priority (100-4096, lower number = higher priority)
- **Default rules**: Cannot be deleted but can be overridden
- **Stateful**: Return traffic is automatically allowed
- **Service Tags**: Predefined groups of IP addresses (e.g., AzureLoadBalancer, Internet)

### Application Security Groups (ASGs)
- Group VMs by application function
- Use in NSG rules instead of explicit IPs
- Simplifies NSG management for complex applications
- VMs can belong to multiple ASGs

### Azure Firewall
- Managed, cloud-based network security service
- Stateful, highly available firewall-as-a-service
- Fixed public IP address
- Application and network level filtering
- Threat intelligence-based filtering
- Must be deployed in its own subnet named "AzureFirewallSubnet" (minimum /26)

### Load Balancers
- **Standard vs Basic**: Standard supports zone redundancy, outbound rules, multiple frontend IPs
- **Public vs Internal**: Public has public IP, Internal operates within VNet
- **SKU cannot be changed** after deployment
- **Health probes**: Required to detect backend endpoint status
- **Session persistence**: Source IP, Source IP + Protocol, or Client IP
- **Floating IP**: Direct server return (DSR) for scenarios like SQL Always On

### Application Gateway
- Layer 7 (HTTP/HTTPS) load balancer
- Web application firewall (WAF) capability
- Cookie-based session affinity
- URL-based routing
- SSL termination
- End-to-end SSL encryption
- Must be in its own subnet

### Azure Front Door
- Global HTTP/HTTPS load balancer with WAF
- Dynamic site acceleration and global HTTP load balancing
- Session affinity
- URL-based routing
- Multiple site hosting
- Operates at Microsoft's edge network

### Traffic Manager
- DNS-based traffic routing
- Global load balancing service
- Does not see the traffic between clients and services
- Routing methods: Priority, Weighted, Performance, Geographic, Subnet, MultiValue

### ExpressRoute
- Private connection to Azure from on-premises
- Doesn't go over the public internet
- Bandwidth options: 50 Mbps to 10 Gbps
- Peering types: Private, Microsoft, and Public (deprecated)
- SKUs: Standard, Premium, Local, Partner
- ExpressRoute Direct: Direct connection to Microsoft's network

### VPN Gateway
- **Types**: Point-to-Site (P2S), Site-to-Site (S2S), VNet-to-VNet
- **SKUs**: Basic, VpnGw1-5, VpnGw1AZ-5AZ (zone-redundant)
- **Active-Active vs Active-Passive**: Active-Active uses both tunnels simultaneously
- Must be deployed in a subnet named "GatewaySubnet" (minimum /27, recommended /26)
- Generation 1 vs Generation 2 gateways (Gen2 has better performance)

## Important Networking Concepts

### IP Addressing
- **Private IP ranges**: 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16
- **Static vs Dynamic**: Static IPs remain the same, Dynamic can change
- **Public IPs**: Can be Standard or Basic SKU (Standard required for zone redundancy)
- **IPv6**: Supported in dual-stack configuration alongside IPv4

### Network Routing
- **System routes**: Default routes automatically created by Azure
- **User-defined routes (UDRs)**: Custom routes that override system routes
- **Next hop types**: Virtual appliance, VNet gateway, VNet, Internet, None
- **Border Gateway Protocol (BGP)**: Used with ExpressRoute and VPN Gateway
- **Route tables**: Collection of user-defined routes applied to subnets

### Service Endpoints
- Extend private VNet address to Azure services
- Traffic stays on Microsoft backbone network
- Available for many services (Storage, SQL, etc.)
- Regional service - can't access services across regions

### Private Link & Private Endpoints
- Private IP addresses for Azure PaaS services
- More secure than service endpoints
- Brings services into your VNet
- Works across regions and across Azure AD tenants
- Supports on-premises access via ExpressRoute/VPN

### Network Watcher
- **Topology**: View resources and relationships
- **Connection Monitor**: Test connectivity between VMs
- **NSG Flow Logs**: Record traffic through NSGs
- **Packet Capture**: Capture network traffic to/from VMs
- **IP Flow Verify**: Test if traffic is allowed/denied
- **Next Hop**: Determine route for a packet

## Networking Limits & Constraints

| Resource | Default Limit | Can be increased? |
|----------|---------------|-------------------|
| VNets per subscription per region | 5,000 | No |
| Subnets per VNet | 3,000 | No |
| Private IP addresses per VNet | 65,536 | No |
| Network interfaces per subscription | 10,000 | Yes |
| NSGs per subscription | 5,000 | No |
| NSG rules per NSG | 1,000 | No |
| UDRs per route table | 400 | No |
| Public IP addresses (Basic) per subscription | 200 | Yes |
| Public IP addresses (Standard) per subscription | 200 | Yes |
| Load balancers per subscription | 1,000 | No |

## Common Networking Scenarios

### Hub and Spoke Topology
- Central hub VNet connected to multiple spoke VNets
- Hub typically contains shared services (firewall, VPN, etc.)
- Spokes contain workloads and connect via peering
- Transitive routing requires UDRs or NVAs

### Forced Tunneling
- Route all internet-bound traffic back to on-premises
- Implemented using UDRs with next hop type of VPN Gateway
- Can be configured on the VPN Gateway

### Network Virtual Appliances (NVAs)
- Third-party network appliances (firewalls, routers, etc.)
- Deployed as VMs in Azure
- Requires UDRs to direct traffic through them
- Consider HA deployment with load balancer

## Exam Tips

- Know the differences between NSGs and Azure Firewall
- Understand VNet peering limitations (no transitive routing)
- Remember subnet naming requirements for special services
- Know when to use each type of load balancer
- Understand routing priority (UDRs > BGP > System routes)
- Know the different VPN Gateway SKUs and their capabilities
- Understand ExpressRoute circuit types and peering options
- Know how to troubleshoot networking issues using Network Watcher
- Understand Private Link vs Service Endpoints
- Remember IP address allocation and reservation rules

## Common PowerShell and Azure CLI Commands

### Virtual Network
```
# Create a VNet
az network vnet create --name MyVNet --resource-group MyRG --address-prefix 10.0.0.0/16

# Create a subnet
az network vnet subnet create --name MySubnet --vnet-name MyVNet --resource-group MyRG --address-prefix 10.0.1.0/24
```

### Network Security Group
```
# Create an NSG
az network nsg create --name MyNSG --resource-group MyRG

# Create an NSG rule
az network nsg rule create --name MyRule --nsg-name MyNSG --resource-group MyRG --priority 100 --direction Inbound --access Allow --protocol Tcp --source-address-prefixes '*' --source-port-ranges '*' --destination-address-prefixes '*' --destination-port-ranges 80
```

### VNet Peering
```
# Create peering from VNet1 to VNet2
az network vnet peering create --name VNet1ToVNet2 --resource-group MyRG --vnet-name VNet1 --remote-vnet VNet2 --allow-vnet-access
```

---

**Note**: This information is AI-generated and should be verified against official Microsoft documentation and other reliable sources before using it for your exam preparation.
