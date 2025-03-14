# Azure Scripts - AZ-104 Cheatsheet

This section contains useful Azure CLI, PowerShell, and Bash scripts for common Azure administration tasks covered in the AZ-104 Microsoft Azure Administrator exam.

## Azure CLI Scripts

### Resource Management

```bash
# List all resource groups
az group list --output table

# Create a resource group
az group create --name MyResourceGroup --location eastus

# Delete a resource group
az group delete --name MyResourceGroup --yes --no-wait

# List resources in a resource group
az resource list --resource-group MyResourceGroup --output table

# Move resources between resource groups
az resource move --destination-group TargetResourceGroup --ids $(az resource list --resource-group SourceResourceGroup --query "[].id" -o tsv)

# Apply a tag to a resource group
az group update --name MyResourceGroup --tags Environment=Production Department=IT

# List all resources with a specific tag
az resource list --tag Environment=Production --output table
```

### Subscription Management

```bash
# List all subscriptions
az account list --output table

# Set active subscription
az account set --subscription "My Subscription"

# Get current subscription details
az account show --output table

# List locations available in your subscription
az account list-locations --output table
```

### Virtual Machines

```bash
# Create a VM with default settings
az vm create --resource-group MyResourceGroup --name MyVM --image UbuntuLTS --admin-username azureuser --generate-ssh-keys

# Create a Windows VM
az vm create --resource-group MyResourceGroup --name MyWindowsVM --image Win2019Datacenter --admin-username azureuser --admin-password "ComplexPassword123!"

# List all VMs
az vm list --output table

# List VM sizes available in a region
az vm list-sizes --location eastus --output table

# Start/Stop VM
az vm start --resource-group MyResourceGroup --name MyVM
az vm stop --resource-group MyResourceGroup --name MyVM
az vm deallocate --resource-group MyResourceGroup --name MyVM

# Resize a VM
az vm resize --resource-group MyResourceGroup --name MyVM --size Standard_DS3_v2

# Add a data disk to a VM
az vm disk attach --resource-group MyResourceGroup --vm-name MyVM --name MyDataDisk --new --size-gb 128

# Run a command on a VM
az vm run-command invoke --resource-group MyResourceGroup --name MyVM --command-id RunShellScript --scripts "apt-get update && apt-get upgrade -y"
```

### Virtual Machine Scale Sets

```bash
# Create a scale set
az vmss create --resource-group MyResourceGroup --name MyScaleSet --image UbuntuLTS --upgrade-policy-mode automatic --admin-username azureuser --generate-ssh-keys --instance-count 3

# Scale out/in manually
az vmss scale --resource-group MyResourceGroup --name MyScaleSet --new-capacity 5

# Enable autoscale
az monitor autoscale create --resource-group MyResourceGroup --resource MyScaleSet --resource-type Microsoft.Compute/virtualMachineScaleSets --name autoscale --min-count 2 --max-count 10 --count 2

# Add an autoscale rule
az monitor autoscale rule create --resource-group MyResourceGroup --autoscale-name autoscale --condition "Percentage CPU > 70 avg 5m" --scale out 2

# Update VMSS instances
az vmss update-instances --resource-group MyResourceGroup --name MyScaleSet --instance-ids "*"
```

### Networking

```bash
# Create a virtual network
az network vnet create --resource-group MyResourceGroup --name MyVNet --address-prefix 10.0.0.0/16 --subnet-name MySubnet --subnet-prefix 10.0.0.0/24

# Create a network security group
az network nsg create --resource-group MyResourceGroup --name MyNSG

# Add NSG rule
az network nsg rule create --resource-group MyResourceGroup --nsg-name MyNSG --name AllowSSH --protocol tcp --priority 1000 --destination-port-range 22 --access allow

# Associate NSG with subnet
az network vnet subnet update --resource-group MyResourceGroup --vnet-name MyVNet --name MySubnet --network-security-group MyNSG

# Create a public IP address
az network public-ip create --resource-group MyResourceGroup --name MyPublicIP --allocation-method Static

# Create a load balancer
az network lb create --resource-group MyResourceGroup --name MyLoadBalancer --frontend-ip-name MyFrontEnd --backend-pool-name MyBackEndPool --public-ip-address MyPublicIP

# Create VNet peering
az network vnet peering create --name VNet1ToVNet2 --resource-group MyResourceGroup --vnet-name VNet1 --remote-vnet VNet2 --allow-vnet-access
```

### Storage

```bash
# Create a storage account
az storage account create --name mystorageaccount --resource-group MyResourceGroup --location eastus --sku Standard_LRS --kind StorageV2

# Create a container
az storage container create --name mycontainer --account-name mystorageaccount --account-key $(az storage account keys list --resource-group MyResourceGroup --account-name mystorageaccount --query "[0].value" -o tsv)

# Upload a file to blob storage
az storage blob upload --container-name mycontainer --file myfile.txt --name myblob.txt --account-name mystorageaccount --account-key $(az storage account keys list --resource-group MyResourceGroup --account-name mystorageaccount --query "[0].value" -o tsv)

# Create a file share
az storage share create --name myfileshare --account-name mystorageaccount --account-key $(az storage account keys list --resource-group MyResourceGroup --account-name mystorageaccount --query "[0].value" -o tsv)

# Generate a SAS token
az storage account generate-sas --account-name mystorageaccount --account-key $(az storage account keys list --resource-group MyResourceGroup --account-name mystorageaccount --query "[0].value" -o tsv) --services bfqt --resource-types sco --permissions acdlrw --expiry $(date -u -d "30 minutes" '+%Y-%m-%dT%H:%MZ')
```

### Identity and Access Management

```bash
# Create a service principal
az ad sp create-for-rbac --name MyServicePrincipal --role Contributor --scopes /subscriptions/$(az account show --query id -o tsv)/resourceGroups/MyResourceGroup

# List role assignments
az role assignment list --resource-group MyResourceGroup --output table

# Assign a role
az role assignment create --assignee user@example.com --role "Virtual Machine Contributor" --resource-group MyResourceGroup

# Create a custom role
az role definition create --role-definition '{
    "Name": "Custom VM Operator",
    "Description": "Can start and stop VMs",
    "Actions": [
        "Microsoft.Compute/virtualMachines/start/action",
        "Microsoft.Compute/virtualMachines/restart/action",
        "Microsoft.Compute/virtualMachines/deallocate/action",
        "Microsoft.Compute/virtualMachines/read"
    ],
    "NotActions": [],
    "AssignableScopes": ["/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"]
}'
```

### Monitoring and Backup

```bash
# Create a Log Analytics workspace
az monitor log-analytics workspace create --resource-group MyResourceGroup --workspace-name MyWorkspace

# Enable diagnostics for a VM
az monitor diagnostic-settings create --resource $(az vm show --resource-group MyResourceGroup --name MyVM --query id -o tsv) --name MyVMDiagnostics --workspace $(az monitor log-analytics workspace show --resource-group MyResourceGroup --workspace-name MyWorkspace --query id -o tsv) --metrics '[{"category": "AllMetrics", "enabled": true}]'

# Create a metric alert
az monitor metrics alert create --name "CPU Alert" --resource-group MyResourceGroup --scopes $(az vm show --resource-group MyResourceGroup --name MyVM --query id -o tsv) --condition "avg Percentage CPU > 90" --window-size 5m --evaluation-frequency 1m

# Create a Recovery Services vault
az backup vault create --name MyVault --resource-group MyResourceGroup --location eastus

# Enable backup for a VM
az backup protection enable-for-vm --resource-group MyResourceGroup --vault-name MyVault --vm $(az vm show -g MyResourceGroup -n MyVM --query id -o tsv) --policy-name DefaultPolicy

# Trigger a backup
az backup protection backup-now --resource-group MyResourceGroup --vault-name MyVault --container-name $(az backup container list -g MyResourceGroup -v MyVault --backup-management-type AzureIaasVM -o tsv --query [0].name) --item-name $(az backup item list -g MyResourceGroup -v MyVault --container-name $(az backup container list -g MyResourceGroup -v MyVault --backup-management-type AzureIaasVM -o tsv --query [0].name) -o tsv --query [0].name) --retain-until 30
```

## PowerShell Scripts

### Resource Management

```powershell
# List all resource groups
Get-AzResourceGroup | Format-Table

# Create a resource group
New-AzResourceGroup -Name "MyResourceGroup" -Location "EastUS"

# Delete a resource group
Remove-AzResourceGroup -Name "MyResourceGroup" -Force

# List resources in a resource group
Get-AzResource -ResourceGroupName "MyResourceGroup" | Format-Table

# Apply a tag to a resource group
Set-AzResourceGroup -Name "MyResourceGroup" -Tag @{Environment="Production"; Department="IT"}

# List all resources with a specific tag
Get-AzResource -TagName "Environment" -TagValue "Production" | Format-Table
```

### Subscription Management

```powershell
# List all subscriptions
Get-AzSubscription | Format-Table

# Set active subscription
Set-AzContext -Subscription "My Subscription"

# Get current subscription details
Get-AzContext | Format-List

# List locations available in your subscription
Get-AzLocation | Format-Table
```

### Virtual Machines

```powershell
# Create a VM
New-AzVm -ResourceGroupName "MyResourceGroup" -Name "MyVM" -Location "EastUS" -VirtualNetworkName "MyVNet" -SubnetName "MySubnet" -SecurityGroupName "MyNSG" -PublicIpAddressName "MyPublicIP" -OpenPorts 80,443,3389

# List all VMs
Get-AzVM | Format-Table

# Start/Stop VM
Start-AzVM -ResourceGroupName "MyResourceGroup" -Name "MyVM"
Stop-AzVM -ResourceGroupName "MyResourceGroup" -Name "MyVM" -Force

# Resize a VM
$vm = Get-AzVM -ResourceGroupName "MyResourceGroup" -VMName "MyVM"
$vm.HardwareProfile.VmSize = "Standard_DS3_v2"
Update-AzVM -VM $vm -ResourceGroupName "MyResourceGroup"

# Add a data disk to a VM
$vm = Get-AzVM -ResourceGroupName "MyResourceGroup" -Name "MyVM"
Add-AzVMDataDisk -VM $vm -Name "MyDataDisk" -DiskSizeInGB 128 -Lun 0 -CreateOption Empty
Update-AzVM -ResourceGroupName "MyResourceGroup" -VM $vm
```

### Networking

```powershell
# Create a virtual network
$subnet = New-AzVirtualNetworkSubnetConfig -Name "MySubnet" -AddressPrefix "10.0.0.0/24"
New-AzVirtualNetwork -Name "MyVNet" -ResourceGroupName "MyResourceGroup" -Location "EastUS" -AddressPrefix "10.0.0.0/16" -Subnet $subnet

# Create a network security group
$nsg = New-AzNetworkSecurityGroup -ResourceGroupName "MyResourceGroup" -Location "EastUS" -Name "MyNSG"

# Add NSG rule
$nsg | Add-AzNetworkSecurityRuleConfig -Name "AllowSSH" -Description "Allow SSH" -Access Allow -Protocol Tcp -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 22 | Set-AzNetworkSecurityGroup

# Associate NSG with subnet
$vnet = Get-AzVirtualNetwork -ResourceGroupName "MyResourceGroup" -Name "MyVNet"
$subnet = Get-AzVirtualNetworkSubnetConfig -Name "MySubnet" -VirtualNetwork $vnet
$subnet.NetworkSecurityGroup = $nsg
$vnet | Set-AzVirtualNetwork
```

### Storage

```powershell
# Create a storage account
New-AzStorageAccount -ResourceGroupName "MyResourceGroup" -Name "mystorageaccount" -Location "EastUS" -SkuName "Standard_LRS" -Kind "StorageV2"

# Get storage account key
$keys = Get-AzStorageAccountKey -ResourceGroupName "MyResourceGroup" -Name "mystorageaccount"
$key = $keys[0].Value

# Create a storage context
$context = New-AzStorageContext -StorageAccountName "mystorageaccount" -StorageAccountKey $key

# Create a container
New-AzStorageContainer -Name "mycontainer" -Context $context -Permission Blob

# Upload a blob
Set-AzStorageBlobContent -File "myfile.txt" -Container "mycontainer" -Blob "myblob.txt" -Context $context

# Create a file share
New-AzStorageShare -Name "myfileshare" -Context $context
```

### Identity and Access Management

```powershell
# Create a service principal
$sp = New-AzADServicePrincipal -DisplayName "MyServicePrincipal"
$BSTR = [System.Runtime.InteropServices.Marshal]::SecureStringToBSTR($sp.Secret)
$plainPassword = [System.Runtime.InteropServices.Marshal]::PtrToStringAuto($BSTR)
Write-Host "App ID: $($sp.ApplicationId)"
Write-Host "App Secret: $plainPassword"

# List role assignments
Get-AzRoleAssignment -ResourceGroupName "MyResourceGroup" | Format-Table

# Assign a role
New-AzRoleAssignment -SignInName "user@example.com" -RoleDefinitionName "Virtual Machine Contributor" -ResourceGroupName "MyResourceGroup"

# Create a custom role
$role = Get-AzRoleDefinition "Virtual Machine Contributor"
$role.Id = $null
$role.Name = "Custom VM Operator"
$role.Description = "Can start and stop VMs"
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Compute/virtualMachines/start/action")
$role.Actions.Add("Microsoft.Compute/virtualMachines/restart/action")
$role.Actions.Add("Microsoft.Compute/virtualMachines/deallocate/action")
$role.Actions.Add("Microsoft.Compute/virtualMachines/read")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx")
New-AzRoleDefinition -Role $role
```

## Bash Scripts for Common Tasks

### Resource Cleanup

````bash
#!/bin/bash
# Script to clean up unused resources

# Set variables
RESOURCE_GROUP="MyResourceGroup"
DAYS_OLD=30

# Find and delete unused disks
echo "Finding unused managed disks..."
UNUSED_DISKS=$(az disk list --query "[?managedBy==
