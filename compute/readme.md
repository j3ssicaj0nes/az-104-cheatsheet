# Azure Compute - AZ-104 Cheatsheet

This section covers Azure compute services, features, and management concepts for the AZ-104 Microsoft Azure Administrator exam.

## Virtual Machines (VMs)

### VM Sizes
- **General Purpose (B, Dsv3, Dv3)**: Balanced CPU-to-memory ratio
- **Compute Optimized (Fsv2)**: High CPU-to-memory ratio
- **Memory Optimized (Esv3, Ev3)**: High memory-to-CPU ratio
- **Storage Optimized (Lsv2)**: High disk throughput and IO
- **GPU (NC, ND, NV)**: Specialized for graphics rendering and visualization
- **High Performance Compute (HB, HC)**: Fastest CPU performance

### VM Series Naming
- **B-series**: Burstable (economical, variable workloads)
- **D-series**: General purpose compute
- **E-series**: Memory optimized
- **F-series**: Compute optimized
- **L-series**: Storage optimized
- **M-series**: Memory optimized (largest memory)
- **N-series**: GPU-enabled

### VM Availability Options
- **Availability Zones**: Physical separation within a region
- **Availability Sets**: Logical grouping within a datacenter
  - **Fault Domains**: Protection from hardware failures (typically 2-3)
  - **Update Domains**: Protection during platform updates (typically 5)
- **Virtual Machine Scale Sets**: Auto-scaling group of identical VMs

### VM Storage
- **OS Disk**: Contains the operating system (default 127 GB)
- **Temporary Disk**: Short-term storage, lost on reboot/resize (SSD)
- **Data Disks**: Additional storage for applications/data
- **Managed Disks**: Azure-managed storage with automatic replication
  - **Standard HDD**: Cost-effective, dev/test workloads
  - **Standard SSD**: Low-latency, production workloads
  - **Premium SSD**: High-performance, IO-intensive workloads
  - **Ultra Disk**: Highest performance, mission-critical workloads

### VM Networking
- **Virtual Network**: Isolated network for VMs
- **Network Interface (NIC)**: Connection to a virtual network
- **Public IP**: Direct internet connectivity
- **Network Security Group (NSG)**: Traffic filtering
- **Application Security Group (ASG)**: Logical grouping for NSG rules
- **Load Balancer**: Distribute traffic across VMs
- **Virtual Network Peering**: Connect VNets

### VM Backup and Disaster Recovery
- **Azure Backup**: Scheduled backups with configurable retention
- **Azure Site Recovery**: Replication to another region for disaster recovery
- **Snapshots**: Point-in-time copy of a disk
- **Managed Disk Snapshots**: Incremental snapshots

### VM Extensions
- **Custom Script Extension**: Run scripts on VMs
- **Desired State Configuration**: Apply configurations
- **VM Diagnostics Extension**: Collect diagnostics data
- **Azure Disk Encryption**: Encrypt OS and data disks
- **Antimalware Extension**: Microsoft Antimalware protection

### VM Creation Options
- **Azure Portal**: GUI-based creation
- **Azure CLI/PowerShell**: Command-line creation
- **ARM Templates**: JSON-based declarative deployment
- **Terraform/Bicep**: Infrastructure as Code
- **VM Images**: Marketplace, custom, shared image gallery

### VM Cost Optimization
- **Reserved Instances**: 1-3 year commitment for discounts (up to 72%)
- **Azure Hybrid Benefit**: Use on-premises licenses in Azure
- **B-series VMs**: Burstable instances for variable workloads
- **Auto-shutdown**: Schedule VM shutdown
- **Rightsizing**: Match VM size to workload requirements

## Virtual Machine Scale Sets (VMSS)

### Key Concepts
- **Instance count**: Number of VM instances
- **Scale set capacity**: Current number of instances
- **Auto-scaling**: Dynamically adjust instance count
- **Scaling policy**: Rules for when to scale in/out
- **Overprovisioning**: Deploy extra VMs to ensure capacity
- **Upgrade policy**: How instances are updated
  - **Manual**: You control when updates happen
  - **Automatic**: Azure updates instances automatically
  - **Rolling**: Updates in batches with optional pause

### Scaling Options
- **Manual scaling**: Manually set instance count
- **Schedule-based scaling**: Predefined schedule
- **Metric-based scaling**: Based on performance metrics
- **Predictive scaling**: ML-based prediction of scaling needs

### Scaling Rules
- **Scale-out**: Add instances when threshold exceeded
- **Scale-in**: Remove instances when below threshold
- **Cool-down period**: Time between scaling actions
- **Common metrics**: CPU, memory, disk, network, queue length

### VMSS Limits
- Up to 1,000 VM instances (default 100)
- Up to 1,000 VMs with Availability Zones
- Up to 100 VMs with Availability Sets
- Up to 600 VMs with Shared Image Gallery

## Azure App Service

### App Service Plans
- **Free/Shared (F1, D1)**: Shared infrastructure, limited features
- **Basic (B1, B2, B3)**: Dedicated VMs, manual scale
- **Standard (S1, S2, S3)**: Auto-scale, staging slots, VNet integration
- **Premium (P1v2-P3v2, P1v3-P3v3)**: Enhanced performance, more slots
- **Isolated (I1-I3)**: Dedicated environment (App Service Environment)

### Deployment Slots
- Test in production-like environment
- Swap between slots with no downtime
- Automatic slot-specific configuration
- Traffic splitting for A/B testing
- Up to 20 slots (Premium/Isolated)

### App Service Networking
- **Public access**: Available from internet
- **Access restrictions**: IP-based restrictions
- **VNet integration**: Connect to Azure VNet
- **Hybrid connections**: Connect to on-premises
- **Private endpoints**: Private IP in your VNet
- **App Service Environment**: Fully isolated environment

### Authentication and Authorization
- **Easy Auth**: Built-in authentication
- **Identity providers**: Microsoft, Google, Facebook, Twitter, OpenID Connect
- **Authentication flow**: Server-directed or client-directed
- **Token store**: Manage tokens for API access

### Continuous Deployment
- **Azure DevOps**: CI/CD pipelines
- **GitHub Actions**: Automated workflows
- **Bitbucket/GitLab**: Repository integration
- **FTP/FTPS**: Manual file upload
- **Zip deploy**: Package deployment
- **Container deployment**: Docker images

### Monitoring and Diagnostics
- **App Service logs**: HTTP logs, application logs
- **Application Insights**: Deep application monitoring
- **Diagnostic logs**: Platform logs
- **Log streaming**: Real-time log viewing
- **Kudu console**: Advanced diagnostics

## Azure Container Instances (ACI)

- Fastest way to deploy containers in Azure
- Per-second billing
- No VM management required
- Linux and Windows containers
- Persistent storage via Azure Files
- Virtual network deployment
- Container groups for multi-container applications
- Resource limits: 4 vCPU, 16 GB RAM per container group

## Azure Kubernetes Service (AKS)

### Key Components
- **Control plane**: Managed by Azure (free)
- **Node pools**: Groups of identical VMs
- **Nodes**: Individual VMs running containers
- **Pods**: One or more containers with shared storage/network
- **Deployments**: Declarative updates to applications
- **Services**: Expose applications internally or externally

### Node Types
- **System node pool**: Runs critical system pods
- **User node pool**: Runs application workloads
- **Virtual nodes**: Serverless containers via ACI

### Scaling Options
- **Manual scaling**: Manually adjust node count
- **Cluster autoscaler**: Automatically adjust node count
- **Horizontal pod autoscaler**: Automatically adjust pod count
- **Vertical pod autoscaler**: Automatically adjust pod resources

### Networking
- **Kubenet**: Basic networking
- **Azure CNI**: Advanced networking with VNet integration
- **Network policies**: Pod-level traffic control
- **Ingress controllers**: HTTP/HTTPS routing
- **Service mesh**: Advanced networking features

### Storage Options
- **Azure Disks**: Block storage for single pod
- **Azure Files**: Shared storage for multiple pods
- **Azure NetApp Files**: High-performance file storage
- **Storage classes**: Define storage types

### AKS Security
- **RBAC**: Role-based access control
- **Azure AD integration**: Identity management
- **Pod identities**: Managed identities for pods
- **Private clusters**: No public endpoints
- **Network policies**: Pod-level firewalls

## Azure Functions

### Hosting Plans
- **Consumption plan**: Serverless, auto-scaling, pay-per-execution
- **Premium plan**: Pre-warmed instances, VNet connectivity, no cold starts
- **App Service plan**: Run on same VMs as web apps

### Triggers and Bindings
- **Triggers**: Events that cause function to run
- **Input bindings**: Data sources for function
- **Output bindings**: Destinations for function output
- **Common triggers**: HTTP, Timer, Blob, Queue, Event Hub, Cosmos DB

### Durable Functions
- **Orchestrator functions**: Define function workflows
- **Activity functions**: Perform work in workflow
- **Entity functions**: Manage state
- **Patterns**: Chaining, Fan-out/fan-in, Async HTTP APIs, Monitoring, Human interaction

### Cold Start
- Time to initialize function after idle period
- Factors: Runtime, dependencies, code size
- Mitigation: Premium plan, keep warm with timer trigger

## Azure Logic Apps

- Visual workflow designer
- 400+ connectors for SaaS and enterprise applications
- Serverless execution model
- Built-in retry policies and error handling
- Enterprise integration capabilities
- B2B scenarios with EDI and XML processing

## Compute Limits & Constraints

| Resource | Limit |
|----------|-------|
| VMs per subscription | 3,500 (can request increase) |
| VMs per region per subscription | 3,500 |
| VMs per availability set | 200 |
| VM disks per VM | 64 |
| Max data disk size | 32,767 GiB |
| VMSS instances | 1,000 (default 100) |
| App Service apps per subscription | 500 |
| Deployment slots per App Service | 20 (Premium/Isolated) |
| ACI container groups per subscription | 100 per region |
| AKS clusters per subscription | 100 |
| AKS nodes per cluster | 1,000 |
| Function apps per subscription | 100 (Consumption), 100 per App Service plan |

## Exam Tips

- Know the differences between VM series and when to use each
- Understand availability sets vs. availability zones
- Know how to configure auto-scaling for VMSS
- Understand App Service plan tiers and features
- Know the networking options for App Service
- Understand container deployment options (ACI vs. AKS)
- Know how to configure AKS networking
- Understand serverless options (Functions vs. Logic Apps)
- Know VM extension types and their purposes
- Understand VM backup and disaster recovery options

## Common PowerShell and Azure CLI Commands

### Virtual Machines
```
# Create a VM
az vm create --resource-group MyResourceGroup --name MyVM --image UbuntuLTS --admin-username azureuser --generate-ssh-keys

# List VMs
az vm list --output table

# Start/Stop VM
az vm start --resource-group MyResourceGroup --name MyVM
az vm stop --resource-group MyResourceGroup --name MyVM

# Resize VM
az vm resize --resource-group MyResourceGroup --name MyVM --size Standard_DS3_v2
```

### Virtual Machine Scale Sets
```
# Create a VMSS
az vmss create --resource-group MyResourceGroup --name MyScaleSet --image UbuntuLTS --upgrade-policy-mode automatic --admin-username azureuser --generate-ssh-keys --instance-count 3

# Scale out/in
az vmss scale --resource-group MyResourceGroup --name MyScaleSet --new-capacity 5

# Update instances
az vmss update-instances --resource-group MyResourceGroup --name MyScaleSet --instance-ids "*"
```

### App Service
```
# Create an App Service plan
az appservice plan create --name MyPlan --resource-group MyResourceGroup --sku S1

# Create a web app
az webapp create --resource-group MyResourceGroup --plan MyPlan --name MyWebApp

# Create a deployment slot
az webapp deployment slot create --name MyWebApp --resource-group MyResourceGroup --slot staging

# Swap deployment slots
az webapp deployment slot swap --name MyWebApp --resource-group MyResourceGroup --slot staging --target-slot production
```

### Azure Kubernetes Service
```
# Create an AKS cluster
az aks create --resource-group MyResourceGroup --name MyAKSCluster --node-count 3 --enable-addons monitoring --generate-ssh-keys

# Get credentials for kubectl
az aks get-credentials --resource-group MyResourceGroup --name MyAKSCluster

# Scale node count
az aks scale --resource-group MyResourceGroup --name MyAKSCluster --node-count 5
```

### Azure Container Instances
```
# Create a container instance
az container create --resource-group MyResourceGroup --name mycontainer --image mcr.microsoft.com/azuredocs/aci-helloworld --dns-name-label aci-demo --ports 80

# List container instances
az container list --output table

# Get container logs
az container logs --resource-group MyResourceGroup --name mycontainer
```

---

**Note**: This information is AI-generated and should be verified against official Microsoft documentation and other reliable sources before using it for your exam preparation.
