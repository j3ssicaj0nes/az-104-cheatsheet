# Azure Resources and Regional Deployment - AZ-104 Cheatsheet

This section provides information about Azure resources, their regional availability, and deployment constraints for the AZ-104 Microsoft Azure Administrator exam.

## Resource Deployment Scope

Azure resources can be deployed at different scopes in the Azure hierarchy:

| Scope Level | Description | Example Resources |
|-------------|-------------|-------------------|
| Management Group | Container for multiple subscriptions | Policies, RBAC assignments |
| Subscription | Container for resource groups | Policies, RBAC assignments, Budgets |
| Resource Group | Container for resources | Most Azure resources |
| Resource | Individual service instance | Resource-specific configurations |

## Regional vs. Global Resources

### Global Resources
These resources are not tied to a specific region and are available globally:

| Resource Type | Notes |
|---------------|-------|
| Azure Active Directory | Single tenant is global |
| Azure DNS | Global service |
| Azure Traffic Manager | Global service |
| Azure Front Door | Global service |
| Azure CDN | Global service with regional edges |
| Azure Policy | Global service |
| Azure RBAC | Global service |
| Management Groups | Global service |

### Regional Resources
These resources must be deployed to a specific Azure region:

| Resource Type | Regional Constraints | Notes |
|---------------|---------------------|-------|
| Virtual Machines | Available in all regions | Some VM sizes are region-specific |
| Virtual Networks | Available in all regions | Global peering available |
| Storage Accounts | Available in all regions | Some features are region-specific |
| App Services | Available in most regions | Some tiers not available in all regions |
| Azure SQL Database | Available in most regions | Some tiers not available in all regions |
| Azure Kubernetes Service | Available in most regions | Some features in preview in select regions |
| Azure Functions | Available in most regions | Consumption plan not available in all regions |
| Azure Logic Apps | Available in most regions | |
| Azure Load Balancer | Available in all regions | Standard SKU required for zone redundancy |
| Application Gateway | Available in most regions | v2 not available in all regions |
| Azure Firewall | Available in most regions | |
| VPN Gateway | Available in most regions | Some SKUs not available in all regions |
| ExpressRoute | Available in select regions | Depends on provider availability |
| Azure Backup | Available in most regions | |
| Azure Site Recovery | Available in most regions | Some source-target combinations restricted |
| Azure Monitor | Available in all regions | Some features are region-specific |
| Azure Cosmos DB | Available in most regions | |
| Azure Redis Cache | Available in most regions | Some tiers not available in all regions |
| Azure Service Bus | Available in most regions | |
| Azure Event Hubs | Available in most regions | |

## Paired Regions

Azure pairs regions for disaster recovery purposes. When service updates are rolled out, only one region in each pair is updated at a time.

| Primary Region | Secondary Region | Notes |
|----------------|------------------|-------|
| East US | West US | North America |
| East US 2 | Central US | North America |
| West US 2 | West Central US | North America |
| North Europe | West Europe | Europe |
| Southeast Asia | East Asia | Asia Pacific |
| UK South | UK West | United Kingdom |
| Australia East | Australia Southeast | Australia |

## Availability Zones

Availability Zones are physically separate locations within an Azure region that are tolerant to local failures.

### Regions with Availability Zones:
- Americas: Central US, East US, East US 2, South Central US, West US 2, Canada Central, Brazil South
- Europe: France Central, Germany West Central, North Europe, UK South, West Europe
- Asia Pacific: Australia East, Japan East, Southeast Asia, East Asia
- Middle East: UAE North

### Zone-redundant Services
Services that support deployment across Availability Zones:

| Service | Zone Redundancy Support | Notes |
|---------|-------------------------|-------|
| Virtual Machines | Yes | Deploy VMs across zones |
| Virtual Machine Scale Sets | Yes | Zone-redundant or zonal deployment |
| Load Balancer (Standard) | Yes | Zone-redundant frontend |
| Public IP Address (Standard) | Yes | Zone-redundant or zonal |
| Managed Disks | Yes | Zone-redundant storage |
| Storage Accounts | Yes | ZRS and GZRS options |
| SQL Database | Yes | Zone-redundant configuration |
| App Service | Yes | Premium v2 and above |
| Event Hubs | Yes | Standard and above |
| Service Bus | Yes | Premium tier |
| Azure Kubernetes Service | Yes | Multiple node pools across zones |
| Azure Cache for Redis | Yes | Premium tier |
| Azure Cosmos DB | Yes | Automatic for multi-region writes |

## Resource Deployment Constraints

### Load Balancer
- **Basic SKU**: Region-specific, cannot span zones
- **Standard SKU**: Can be zone-redundant or zonal
- **Internal Load Balancer**: Must be in same VNet as backend pool
- **Public Load Balancer**: Can serve resources in any region if using Global tier

### Application Gateway
- Must be deployed in its own subnet
- v1 SKU: No zone redundancy
- v2 SKU: Supports zone redundancy in select regions
- WAF SKU: Available in most regions

### Virtual Network
- Regional resource
- Can connect to other VNets in same or different regions via peering
- Can connect to on-premises via VPN Gateway or ExpressRoute
- Address space cannot overlap with connected networks

### Storage Account
- Regional resource (except for GRS/GZRS which replicate to a secondary region)
- Some features only available in specific regions:
  - NFS 3.0 for Azure Files: Limited regions
  - Zone-redundant storage: Only in regions with Availability Zones
  - Premium file shares: Not available in all regions

### Virtual Machines
- Regional resource
- Size availability varies by region
- Specialized VMs (GPU, HPC) available only in select regions
- Generation 2 VMs not available in all regions
- Spot VMs not available in all regions

### Azure Kubernetes Service
- Regional resource
- Some features only available in specific regions:
  - Private clusters: Limited availability
  - Network policies: Limited availability
  - Virtual nodes: Limited availability

### Azure SQL Database
- Regional resource
- Hyperscale tier not available in all regions
- Serverless tier not available in all regions
- Zone-redundancy requires Business Critical tier

### Azure Backup
- Recovery Services vault is a regional resource
- Cross-region restore available for Azure VMs in select regions
- Long-term retention varies by workload type

### Azure Site Recovery
- Recovery Services vault is a regional resource
- Some source-target region combinations may not be supported
- Not all VM sizes supported for all region combinations

## Resource Group Considerations

- Resource groups are regional, but can contain resources from any region
- Best practice: Group resources that share the same lifecycle
- Best practice: Place resources in the same region as their resource group when possible
- Resource groups cannot be renamed
- Resources can be moved between resource groups
- Moving resources may cause downtime for some resource types

## Resource Naming Constraints

| Resource Type | Character Limit | Naming Restrictions |
|---------------|----------------|---------------------|
| Resource Group | 1-90 | Alphanumeric, underscore, parentheses, hyphen, period (except at end) |
| Virtual Machine | 1-64 | Alphanumeric, hyphen |
| Storage Account | 3-24 | Lowercase letters and numbers only |
| Azure SQL Server | 1-63 | Lowercase letters, numbers, and hyphens. Cannot start or end with hyphen |
| Azure SQL Database | 1-128 | Cannot use: <>*%&:\/? or control characters |
| Public IP | 1-80 | Alphanumeric, underscore, hyphen, period |
| Virtual Network | 2-64 | Alphanumeric, underscore, hyphen, period |
| Network Interface | 1-80 | Alphanumeric, underscore, hyphen, period |
| Network Security Group | 1-80 | Alphanumeric, underscore, hyphen, period |
| App Service | 2-60 | Alphanumeric and hyphens |
| Function App | 2-60 | Alphanumeric and hyphens |
| Azure Kubernetes Service | 1-63 | Alphanumeric and hyphens |

## Region Selection Factors

When choosing a region for your resources, consider:

1. **Data Residency & Compliance**: Legal and regulatory requirements
2. **Service Availability**: Not all services available in all regions
3. **Performance**: Choose regions closest to users
4. **Cost**: Pricing varies by region
5. **Disaster Recovery**: Use paired regions for redundancy
6. **Availability Zones**: Choose regions with AZs for high availability

## Common PowerShell and Azure CLI Commands

### Check Resource Providers and Locations

```bash
# List all resource providers
az provider list --output table

# List all locations
az account list-locations --output table

# Check if a service is available in a region
az provider show --namespace Microsoft.Compute --query "resourceTypes[?resourceType=='virtualMachines'].locations" --output table
```

```powershell
# List all resource providers
Get-AzResourceProvider | Format-Table

# List all locations
Get-AzLocation | Format-Table

# Check if a service is available in a region
Get-AzResourceProvider -ProviderNamespace Microsoft.Compute | Where-Object {$_.ResourceTypes.ResourceTypeName -eq "virtualMachines"} | Select-Object -ExpandProperty ResourceTypes | Select-Object ResourceTypeName, Locations
```

### Check Resource Constraints

```bash
# Check VM sizes available in a region
az vm list-sizes --location eastus --output table

# Check VM image availability in a region
az vm image list --location eastus --output table --all

# Check storage account SKUs available in a region
az storage account create --help | grep --after-context=20 "--sku"
```

```powershell
# Check VM sizes available in a region
Get-AzVMSize -Location "East US" | Format-Table

# Check VM image availability in a region
Get-AzVMImagePublisher -Location "East US" | Get-AzVMImageOffer | Get-AzVMImageSku | Format-Table

# Check storage account SKUs available
Get-AzStorageSkuName | Format-Table
```

---

**Note**: This information is AI-generated and should be verified against official Microsoft documentation and other reliable sources before using it for your exam preparation. Azure services and regional availability change frequently.
