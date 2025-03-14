# Azure Storage - AZ-104 Cheatsheet

This section covers Azure storage services, features, and management concepts for the AZ-104 Microsoft Azure Administrator exam.

## Storage Accounts

### Storage Account Types
- **Standard general-purpose v2 (GPv2)**: Blob, File, Queue, Table storage
- **Premium block blob**: High-performance block blob storage
- **Premium file shares**: High-performance file storage
- **Premium page blob**: High-performance page blob storage

### Performance Tiers
- **Standard**: HDD-backed storage, lower cost, higher latency
- **Premium**: SSD-backed storage, higher cost, lower latency

### Access Tiers
- **Hot**: Frequently accessed data, highest storage cost, lowest access cost
- **Cool**: Infrequently accessed data (30+ days), lower storage cost, higher access cost
- **Archive**: Rarely accessed data (180+ days), lowest storage cost, highest access cost, retrieval latency

### Replication Options
- **Locally Redundant Storage (LRS)**: 3 copies in single datacenter
- **Zone-Redundant Storage (ZRS)**: 3 copies across availability zones in one region
- **Geo-Redundant Storage (GRS)**: 6 copies (3 in primary region, 3 in secondary region)
- **Geo-Zone-Redundant Storage (GZRS)**: 6 copies (3 across zones in primary region, 3 in secondary region)
- **Read-Access Geo-Redundant Storage (RA-GRS)**: GRS with read access to secondary region
- **Read-Access Geo-Zone-Redundant Storage (RA-GZRS)**: GZRS with read access to secondary region

### Networking & Security
- **Public access**: Available from all networks
- **Selected networks**: Restrict to specific VNets and IP ranges
- **Private endpoints**: Dedicated private IP within VNet
- **Service endpoints**: Keep traffic on Azure backbone
- **Shared Access Signatures (SAS)**: Limited access with expiration
- **Storage account keys**: Full access to storage account
- **Azure AD integration**: Role-based access control

### Lifecycle Management
- Automatically transition blobs between tiers
- Delete blobs after specified time period
- Rule conditions: creation time, last access time, blob type
- Actions: move to cool/archive tier, delete blob

## Blob Storage

### Blob Types
- **Block blobs**: Text and binary data, up to 4.75 TB
- **Page blobs**: Random access files, up to 8 TB, used for VHD files
- **Append blobs**: Optimized for append operations, up to 195 GB

### Access Tiers (Blob-level)
- Can set access tier at blob level (overrides account default)
- Minimum storage duration: Cool (30 days), Archive (180 days)
- Archive retrieval options: Standard (up to 15 hours), High (1-12 hours), Priority (under 1 hour)

### Blob Storage Features
- **Soft delete**: Recover deleted blobs within retention period (1-365 days)
- **Versioning**: Automatically maintain previous versions of blobs
- **Change feed**: Track changes to blobs
- **Blob inventory**: Scheduled reports on blob data
- **Static website hosting**: Host static content directly from blob storage
- **Object replication**: Asynchronously copy block blobs between accounts
- **Blob indexing**: Add key-value attributes to blobs for filtering

## Azure Files

### File Share Types
- **Standard**: HDD-based storage
- **Premium**: SSD-based storage, higher IOPS and throughput

### Protocols
- **SMB 3.0/3.1**: Windows, Linux, macOS
- **NFS 4.1**: Linux (Premium only)
- **REST API**: Programmatic access

### Features
- **Azure File Sync**: Cache file shares on Windows Server
- **Snapshots**: Point-in-time read-only copies
- **Soft delete**: Recover deleted files within retention period
- **Large file shares**: Up to 100 TiB
- **Identity-based authentication**: Azure AD Kerberos, AD Domain Services
- **Encryption**: Encryption at rest, in transit

### Azure File Sync Components
- **Storage Sync Service**: Top-level Azure resource
- **Sync group**: Group defining topology between endpoints
- **Registered server**: Server with Storage Sync Agent
- **Server endpoint**: Specific location on registered server
- **Cloud endpoint**: Azure file share

## Table Storage

- NoSQL key-attribute data store
- Schema-less design
- Key properties: PartitionKey and RowKey
- Entity size limit: 1 MB
- Table size limit: 500 TB
- Query capabilities: Point queries, range queries on RowKey

## Queue Storage

- Store and retrieve messages
- Message size limit: 64 KB
- Queue size limit: 500 TB
- Message TTL: Up to 7 days
- Visibility timeout: Lock message during processing
- De-queue count: Track processing attempts

## Storage Security

### Authentication Methods
- **Shared Key**: Storage account keys
- **Shared Access Signature (SAS)**:
  - **Account SAS**: Access to multiple storage services
  - **Service SAS**: Access to specific storage service
  - **User delegation SAS**: Secured with Azure AD credentials
- **Azure AD**: Role-based access control

### Encryption
- **Encryption at rest**: All data automatically encrypted
- **Encryption in transit**: HTTPS/TLS
- **Customer-managed keys**: Bring your own encryption keys
- **Double encryption**: Infrastructure and service-level encryption

### Network Security
- **Firewall rules**: IP and VNET restrictions
- **Private endpoints**: Private IP in your VNet
- **Service endpoints**: Keep traffic on Azure backbone

## Data Protection

### Backup Options
- **Soft delete**: Recover deleted data
- **Blob versioning**: Maintain previous versions
- **Blob snapshots**: Point-in-time copies
- **File share snapshots**: Point-in-time copies
- **Azure Backup**: Integrated backup service

### Immutable Storage
- **Legal hold**: Simple immutability, no expiration
- **Time-based retention**: Specify retention interval
- **WORM (Write Once, Read Many)**: Prevent modification/deletion

## Storage Monitoring

- **Metrics**: Performance, capacity, transactions, availability
- **Diagnostic logs**: Requests, authentication, etc.
- **Azure Monitor**: Alerts, dashboards
- **Storage Explorer**: Visual management tool
- **AzCopy**: Command-line utility for data transfer

## Storage Limits & Constraints

| Resource | Limit |
|----------|-------|
| Max storage account size | 5 PB |
| Max blob size (block) | 4.75 TB |
| Max blob size (page) | 8 TB |
| Max file share size | 100 TiB (standard), 100 TiB (premium) |
| Max file size | 1 TiB |
| Max number of containers | Unlimited |
| Max number of blobs per container | Unlimited |
| Max IOPS per standard account | 20,000 |
| Max ingress per standard account | 10 Gbps |
| Max egress per standard account | 50 Gbps |

## Data Transfer Options

- **AzCopy**: Command-line utility
- **Storage Explorer**: GUI tool
- **Azure File Sync**: For file shares
- **Azure Data Box**: Physical device for offline transfer
- **Azure Import/Export service**: Ship physical drives
- **Azure Data Factory**: ETL service

## Cost Optimization

- Choose appropriate access tier for data
- Use lifecycle management to automatically tier data
- Select appropriate redundancy level
- Use reserved capacity for predictable workloads
- Monitor and right-size storage accounts
- Delete unneeded data and snapshots
- Use compression where possible

## Exam Tips

- Know the differences between storage account types and when to use each
- Understand replication options and their disaster recovery capabilities
- Know how to secure storage accounts (network, authentication, encryption)
- Understand lifecycle management rules and conditions
- Know the differences between blob types and their use cases
- Understand Azure File Sync topology and components
- Know SAS token types and their use cases
- Understand soft delete, versioning, and snapshots
- Know how to monitor and troubleshoot storage accounts

## Common PowerShell and Azure CLI Commands

### Storage Account Management
```
# Create a storage account
az storage account create --name mystorageaccount --resource-group MyResourceGroup --location eastus --sku Standard_LRS

# List storage accounts
az storage account list --output table

# Get storage account keys
az storage account keys list --account-name mystorageaccount --resource-group MyResourceGroup
```

### Blob Storage
```
# Create a container
az storage container create --name mycontainer --account-name mystorageaccount --account-key <account-key>

# Upload a blob
az storage blob upload --container-name mycontainer --name myblob --file myfile.txt --account-name mystorageaccount --account-key <account-key>

# List blobs
az storage blob list --container-name mycontainer --account-name mystorageaccount --account-key <account-key>
```

### File Shares
```
# Create a file share
az storage share create --name myfileshare --account-name mystorageaccount --account-key <account-key>

# Upload a file
az storage file upload --share-name myfileshare --source myfile.txt --path mypath/myfile.txt --account-name mystorageaccount --account-key <account-key>

# List files
az storage file list --share-name myfileshare --path mypath --account-name mystorageaccount --account-key <account-key>
```

### SAS Tokens
```
# Create a service SAS token for a blob
az storage blob generate-sas --container-name mycontainer --name myblob --permissions r --expiry 2023-01-01T00:00:00Z --account-name mystorageaccount --account-key <account-key>

# Create an account SAS token
az storage account generate-sas --permissions cdlruwap --resource-types sco --services bfqt --expiry 2023-01-01T00:00:00Z --account-name mystorageaccount --account-key <account-key>
```

---

**Note**: This information is AI-generated and should be verified against official Microsoft documentation and other reliable sources before using it for your exam preparation.
