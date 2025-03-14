# Azure Monitoring and Backup - AZ-104 Cheatsheet

This section covers Azure monitoring, logging, and backup services for the AZ-104 Microsoft Azure Administrator exam.

## Azure Monitor

### Key Components
- **Metrics**: Numerical values collected at regular intervals
- **Logs**: Text-based data with detailed information
- **Alerts**: Notifications based on metrics or logs
- **Application Insights**: Application monitoring
- **VM Insights**: VM performance monitoring
- **Container Insights**: Container monitoring
- **Network Insights**: Network monitoring

### Data Sources
- **Application data**: Telemetry from applications
- **OS data**: Windows and Linux performance counters
- **Azure resource data**: Metrics from Azure services
- **Azure subscription data**: Service health, activity logs
- **Azure tenant data**: Azure AD logs

### Data Platform
- **Metrics database**: Time-series database optimized for performance analysis
- **Log Analytics**: Query and analyze log data
- **Application Insights**: Application-specific monitoring
- **Dashboards**: Customizable visualization
- **Workbooks**: Interactive reports

### Metrics
- Lightweight numerical values collected at 1-minute intervals
- Stored for 93 days
- Near real-time monitoring
- Good for alerting and dashboards
- Dimensions allow filtering and segmentation
- Custom metrics via API

### Logs
- Stored in Log Analytics workspace
- Retention configurable (30-730 days)
- Query using Kusto Query Language (KQL)
- Log categories:
  - Activity logs: Control-plane operations
  - Resource logs: Data-plane operations
  - Azure AD logs: Identity operations
  - VM guest logs: OS and application logs

### Log Analytics
- Central repository for logs
- Cross-resource queries
- Workspace-based access control
- Data retention configuration
- Query performance optimization
- Workspace design considerations:
  - Single workspace vs. multiple workspaces
  - Access control requirements
  - Data sovereignty/compliance

### Alerts
- **Metric alerts**: Based on metric values
- **Log alerts**: Based on log query results
- **Activity log alerts**: Based on Azure Activity Log events
- **Smart detection alerts**: ML-based anomaly detection
- **Action groups**: Define notification recipients and actions
- **Alert processing rules**: Centrally manage alert handling

### Alert Components
- **Alert rule**: Condition that triggers an alert
- **Action group**: Set of actions to take when alert fires
- **Alert state**: Fired, Acknowledged, Closed
- **Smart groups**: ML-based alert grouping

### Workbooks
- Interactive reports combining text, metrics, logs, and parameters
- Shareable and can be pinned to dashboards
- Templates for common scenarios
- Custom visualizations

## Azure Service Health

### Components
- **Azure Status**: Global Azure health
- **Service Health**: Personalized view of service health
- **Resource Health**: Health of specific resources

### Service Health Categories
- **Service issues**: Problems affecting Azure services
- **Planned maintenance**: Scheduled maintenance activities
- **Health advisories**: Changes requiring your action
- **Security advisories**: Security-related issues

### Health Alerts
- Create alerts for service health events
- Configure by service, region, and event type
- Integrate with action groups

## Azure Backup

### Key Concepts
- **Recovery Services vault**: Container for backup data
- **Backup policy**: Schedule and retention settings
- **Backup instance**: Individual item being backed up
- **Recovery point**: Point-in-time backup snapshot
- **Soft delete**: Protection against accidental/malicious deletion

### Supported Workloads
- **Azure VMs**: Full VM backup
- **SQL Server in Azure VMs**: Database-level backup
- **SAP HANA in Azure VMs**: Database-level backup
- **Azure Files**: File share backup
- **Azure Blobs**: Blob backup
- **On-premises**: Files, folders, system state via MARS agent
- **On-premises VMware/Hyper-V**: VM backup via DPM/MABS

### Backup Components
- **Recovery Services vault**: Central entity for backups
- **Backup policy**: Defines schedule and retention
- **MARS agent**: Microsoft Azure Recovery Services agent for file/folder backup
- **DPM/MABS**: System Center Data Protection Manager/Microsoft Azure Backup Server
- **Backup extension**: For Azure VM backup

### Backup Types
- **Full backup**: Complete copy of data
- **Incremental backup**: Changes since last backup
- **Differential backup**: Changes since last full backup
- **Application-consistent**: Uses VSS for application consistency
- **File-consistent**: Crash-consistent backup
- **Crash-consistent**: Like taking a snapshot while VM is running

### Retention Options
- Daily, weekly, monthly, yearly recovery points
- GFS (Grandfather-Father-Son) retention scheme
- Minimum retention: 7 days
- Maximum retention: 9999 days (27+ years)

### Backup Security
- **Storage encryption**: Backup data encrypted at rest
- **Transit encryption**: Data encrypted during transfer
- **Authentication**: Azure AD authentication
- **RBAC**: Role-based access control
- **Soft delete**: 14-day protection against deletion
- **Cross-region restore**: Restore to secondary region

## Azure Site Recovery (ASR)

### Key Concepts
- **Recovery Services vault**: Container for replication data
- **Replication policy**: Frequency and recovery point settings
- **Recovery plan**: Orchestration for failover
- **RTO (Recovery Time Objective)**: Time to restore service
- **RPO (Recovery Point Objective)**: Maximum data loss

### Supported Scenarios
- **Azure VM to Azure region**: Region-to-region DR
- **VMware to Azure**: On-premises to cloud DR
- **Hyper-V to Azure**: On-premises to cloud DR
- **Physical servers to Azure**: On-premises to cloud DR

### ASR Components
- **Configuration server**: Manages replication for VMware/physical
- **Process server**: Replication data handling
- **Master target server**: Handles failback
- **Mobility service**: Installed on source servers
- **Recovery Services vault**: Central management

### Replication Process
1. Initial replication: Full copy of source
2. Delta replication: Ongoing changes
3. Recovery points: Point-in-time snapshots
4. Failover: Switch to target environment
5. Failback: Return to source environment

### Network Mapping
- Map source networks to target networks
- Configure target network settings
- IP addressing options:
  - Same IP as source
  - Different IP than source

### Recovery Plans
- Ordered grouping of machines
- Custom scripts and manual actions
- Group machines by application tiers
- Test failover without disruption

## Azure Monitor for VMs

### Components
- **VM Insights**: Performance and dependency monitoring
- **Log Analytics agent**: Collects logs and performance data
- **Dependency agent**: Discovers processes and dependencies
- **Performance metrics**: CPU, memory, disk, network
- **Map**: Visual representation of VM dependencies

### Monitoring Capabilities
- **Performance**: CPU, memory, disk, network metrics
- **Processes**: Running processes and dependencies
- **Dependencies**: Inbound and outbound connections
- **Alerts**: Based on performance thresholds
- **Workbooks**: Pre-built reports for analysis

### Agent Installation
- **Log Analytics agent**: Windows and Linux
- **Dependency agent**: Requires Log Analytics agent
- **Azure Arc**: For non-Azure VMs
- **At scale**: Via Azure Policy

## Network Monitoring

### Network Watcher
- **Topology**: Visual network layout
- **Connection Monitor**: Test connectivity
- **NSG flow logs**: Record network traffic
- **Packet capture**: Capture network packets
- **IP flow verify**: Test if traffic is allowed
- **Next hop**: Determine route for a packet
- **Effective security rules**: View combined NSG rules
- **VPN troubleshooting**: Diagnose VPN issues

### NSG Flow Logs
- Record inbound/outbound flows through NSGs
- Version 2 includes: 5-tuple, direction, decision, timestamps
- Stored in storage account
- Can be analyzed with Traffic Analytics

### Traffic Analytics
- Analyzes NSG flow logs
- Provides insights on traffic patterns
- Identifies security threats
- Visualizes traffic flows
- Stored in Log Analytics workspace

## Monitoring Limits & Constraints

| Resource | Limit |
|----------|-------|
| Metrics retention | 93 days |
| Log retention (free tier) | 7-30 days |
| Log retention (paid tier) | Up to 730 days |
| Log Analytics query timeout | 30 minutes |
| Log Analytics query results | 30,000 rows or 8 MB |
| Alert rules per subscription | 512 |
| Action groups per subscription | 2,000 |
| Recovery Services vaults per subscription | 500 |
| Backups per day per vault | 100 (Azure VMs) |
| Recovery points per protected instance | 9,999 |

## Exam Tips

- Know the differences between metrics and logs
- Understand Log Analytics workspace design considerations
- Know how to create and configure alerts
- Understand the different backup types and when to use each
- Know the components of Azure Site Recovery
- Understand Network Watcher capabilities and use cases
- Know how to monitor VMs using VM Insights
- Understand the differences between Azure Backup and Azure Site Recovery
- Know how to configure backup policies and retention
- Understand soft delete and its purpose

## Common PowerShell and Azure CLI Commands

### Azure Monitor
```
# Create a Log Analytics workspace
az monitor log-analytics workspace create --resource-group MyResourceGroup --workspace-name MyWorkspace

# List Log Analytics workspaces
az monitor log-analytics workspace list --output table

# Run a Log Analytics query
az monitor log-analytics query --workspace MyWorkspace --analytics-query "Heartbeat | summarize count() by Computer | limit 10"
```

### Alerts
```
# Create a metric alert
az monitor metrics alert create --name "CPU Alert" --resource-group MyResourceGroup --scopes "/subscriptions/{subscriptionId}/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/virtualMachines/MyVM" --condition "avg Percentage CPU > 90" --window-size 5m --evaluation-frequency 1m

# List alerts
az monitor metrics alert list --resource-group MyResourceGroup
```

### Azure Backup
```
# Create a Recovery Services vault
az backup vault create --name MyVault --resource-group MyResourceGroup --location eastus

# Enable backup for an Azure VM
az backup protection enable-for-vm --resource-group MyResourceGroup --vault-name MyVault --vm MyVM --policy-name DefaultPolicy

# Trigger a backup
az backup protection backup-now --resource-group MyResourceGroup --vault-name MyVault --container-name MyVM --item-name MyVM --retain-until 01-01-2023
```

### Azure Site Recovery
```
# Create a Recovery Services vault for ASR
az site-recovery vault create --name MyASRVault --resource-group MyResourceGroup --location eastus

# Enable replication for an Azure VM
az site-recovery protection enable-for-azure-vm --resource-group MyResourceGroup --vault-name MyASRVault --vm MyVM --target-zone "2" --target-resource-group MyTargetRG --target-vnet MyTargetVNet
```

### Network Watcher
```
# Enable Network Watcher in a region
az network watcher configure --resource-group NetworkWatcherRG --locations eastus westus --enabled true

# Capture packets
az network watcher packet-capture create --name MyCapture --vm MyVM --resource-group MyResourceGroup --time-limit 60

# Test connectivity
az network watcher connectivity check --source-resource MyVM --source-resource-group MyResourceGroup --destination-resource MyTargetVM --destination-resource-group MyTargetRG
```

---

**Note**: This information is AI-generated and should be verified against official Microsoft documentation and other reliable sources before using it for your exam preparation.
