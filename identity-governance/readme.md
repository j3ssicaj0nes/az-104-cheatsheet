# Azure Identity and Governance - AZ-104 Cheatsheet

This section covers Azure identity services, governance concepts, and subscription management topics for the AZ-104 Microsoft Azure Administrator exam.

## Azure Active Directory (Azure AD)

### Key Concepts
- **Tenant**: Dedicated instance of Azure AD representing an organization
- **User**: Identity with access to Azure resources and applications
- **Group**: Collection of users for simplified access management
- **Service Principal**: Identity for applications/services
- **Managed Identity**: Azure-managed service principal for Azure resources
- **Device**: Physical or virtual device registered with Azure AD

### User Types
- **Cloud identity**: Created directly in Azure AD
- **Directory-synchronized identity**: Synced from on-premises AD
- **Guest user**: External user invited to your tenant

### Authentication Methods
- **Password**: Traditional username/password
- **Multi-Factor Authentication (MFA)**: Combines multiple verification methods
- **Passwordless**: FIDO2 keys, Microsoft Authenticator, Windows Hello
- **Conditional Access**: Context-based access control

### Azure AD Editions
- **Free**: Basic user and group management, SSO
- **Office 365 Apps**: Self-service password reset, customized branding
- **Premium P1**: Dynamic groups, conditional access, self-service group management
- **Premium P2**: Identity Protection, Privileged Identity Management (PIM)

### Azure AD Connect
- Synchronizes on-premises AD with Azure AD
- **Sync methods**: Password hash sync, Pass-through authentication, Federation
- **Filtering options**: Domain-based, OU-based, Attribute-based
- **Hybrid Identity**: Extends on-premises identities to the cloud

## Role-Based Access Control (RBAC)

### Key Components
- **Security Principal**: User, group, service principal, or managed identity
- **Role Definition**: Collection of permissions (actions/notactions)
- **Scope**: Boundary where access applies
- **Role Assignment**: Attachment of a role definition to a security principal at a scope

### Built-in Roles
- **Owner**: Full access including assigning access to others
- **Contributor**: Full access excluding assigning access to others
- **Reader**: View-only access
- **User Access Administrator**: Manage user access to Azure resources
- **Service-specific roles**: Virtual Machine Contributor, Network Contributor, etc.

### Scope Hierarchy
1. **Management Group**: Container for multiple subscriptions
2. **Subscription**: Container for resource groups
3. **Resource Group**: Container for resources
4. **Resource**: Individual service instance

### RBAC Best Practices
- Use built-in roles when possible
- Apply roles at the highest appropriate scope
- Use groups instead of individual assignments
- Limit the number of Owner role assignments
- Use Privileged Identity Management for just-in-time access

## Azure Subscriptions and Management Groups

### Subscription Types
- **Free**: $200 credit for 30 days, limited services
- **Pay-As-You-Go**: Pay for what you use
- **Enterprise Agreement (EA)**: Volume licensing agreement
- **CSP**: Purchased through Cloud Solution Provider
- **Visual Studio/MSDN**: For developers with Visual Studio subscriptions

### Subscription Limits
- 980 resource groups per subscription
- 800 resources per resource group
- 50 management groups per tenant
- 10,000 management group hierarchy depth

### Management Groups
- Organize subscriptions into containers
- Apply governance conditions (policies, access) to multiple subscriptions
- Up to 6 levels of depth (excluding root and subscription levels)
- Inheritance of policies and RBAC from parent to child

### Subscription Management
- **Move between tenants**: Requires ownership of both subscriptions
- **Transfer billing ownership**: Changes the billing account
- **Change subscription type**: Convert between different offers

## Azure Policy

### Key Components
- **Policy Definition**: Rule for specific conditions
- **Initiative Definition**: Group of policy definitions
- **Assignment**: Application of policy/initiative to a scope
- **Compliance**: Resource state relative to policy requirements
- **Exemption**: Exclusion from policy evaluation

### Effect Types
- **Audit**: Log non-compliant resources but don't enforce
- **Deny**: Prevent non-compliant resource creation/modification
- **Append**: Add specified information to resource
- **Modify**: Add, update, or remove properties on a resource
- **DeployIfNotExists**: Deploy related resources if they don't exist
- **AuditIfNotExists**: Audit if related resources don't exist

### Built-in Policies
- Allowed locations/regions
- Allowed resource types
- Allowed VM SKUs
- Require tags and their values
- Inherit tags from subscription/resource group

### Policy Evaluation
- Triggered on resource creation/update
- Background compliance scanning (typically every 24 hours)
- Remediation tasks for non-compliant resources

## Azure Blueprints

- Templates that package artifacts: policies, RBAC, ARM templates, resource groups
- **Blueprint definition**: What should be deployed
- **Blueprint assignment**: Deploying the blueprint to a scope
- **Versioning**: Track changes to blueprint definitions
- **Locking**: Prevent unauthorized changes to deployed resources

## Resource Tags

- Key-value pairs for organizing resources
- Not inherited by default (unless using policy)
- 50 tags per resource/resource group
- Tag name: 512 characters max
- Tag value: 256 characters max
- Cannot be applied to classic resources

## Azure Resource Manager (ARM)

- Control plane for all Azure resource operations
- Handles authentication, authorization, and resource provisioning
- **Resource Providers**: Services that supply Azure resources
- **ARM Templates**: JSON files for declarative deployment
- **Deployment Modes**: Incremental vs. Complete
- **Template Specs**: Store ARM templates as resources

## Resource Locks

- **Read-only (ReadOnly)**: Prevent modification
- **Delete (CanNotDelete)**: Prevent deletion but allow modifications
- Applied at subscription, resource group, or resource level
- Inherited by child resources
- Require appropriate permissions to add/remove

## Cost Management

- **Cost Analysis**: Analyze historical spending
- **Budgets**: Set spending thresholds with alerts
- **Advisor Recommendations**: Cost optimization suggestions
- **Pricing Calculator**: Estimate costs before deployment
- **TCO Calculator**: Compare on-premises vs. Azure costs
- **Reserved Instances**: Discounted pricing for 1-3 year commitments
- **Azure Hybrid Benefit**: Use on-premises licenses in Azure

## Exam Tips

- Know the differences between Azure AD editions
- Understand RBAC inheritance and scope hierarchy
- Know how to calculate effective permissions across multiple role assignments
- Understand policy effects and when to use each
- Know how to implement governance across multiple subscriptions
- Understand the relationship between management groups, subscriptions, and resource groups
- Know how to use tags effectively for organization and cost tracking
- Understand resource locking mechanisms and their implications

## Common PowerShell and Azure CLI Commands

### Azure AD
```
# Create a new Azure AD user
az ad user create --display-name "John Doe" --password "Password123" --user-principal-name john.doe@contoso.com

# List Azure AD groups
az ad group list --output table
```

### RBAC
```
# Assign a role to a user
az role assignment create --role "Contributor" --assignee john.doe@contoso.com --resource-group MyResourceGroup

# List role assignments
az role assignment list --resource-group MyResourceGroup
```

### Subscriptions
```
# List subscriptions
az account list --output table

# Set active subscription
az account set --subscription "My Subscription"
```

### Azure Policy
```
# Assign a policy
az policy assignment create --name "restrict-location" --policy "e56962a6-4747-49cd-b67b-bf8b01975c4c" --params "{ \"listOfAllowedLocations\": { \"value\": [\"eastus\", \"westus\"] } }"

# Check policy compliance
az policy state list --resource-group MyResourceGroup
```

---

**Note**: This information is AI-generated and should be verified against official Microsoft documentation and other reliable sources before using it for your exam preparation.