# DX - Azure App Service Module

This module deploys an AppService with a strong opinionated configuration in terms of networking and deployment strategy. This module is ideal when you need your AppService to remain inaccessible from the public internet.

## Features

- **AppService**: An AppService instance running Java or TypeScript code
- **AppService Slot**: A slot named `Staging` to test code before switching to production
- **App Service Plan**: A Linux-based Plan
- **Private Endpoint**: To allow only private incoming connections
- **Subnet**: To allow outbound connections in a VNet

## Tiers and Configuration

| Tier | Description                      | SLA    | Staging Slot | Autoscaling | Multi AZ |
| ---- | -------------------------------- | ------ | ------------ | ----------- | -------- |
| s    | Non-production tier              | 99.95% | No           | Max 3       | No       |
| m    | Standard production tier         | 99.95% | Yes          | Max 30      | Yes      |
| l    | Above average production tier    | 99.95% | Yes          | Max 30      | Yes      |
| xl   | High-performance production tier | 99.95% | Yes          | Max 30      | Yex      |

## Usage Example

For a complete example of how to use this module, refer to the [examples/complete](https://github.com/pagopa-dx/terraform-azurerm-azure-app-service/tree/main/examples/complete) folder in the module repository.

<!-- markdownlint-disable -->
<!-- BEGIN_TF_DOCS -->
## Requirements

| Name | Version |
|------|---------|
| <a name="requirement_azurerm"></a> [azurerm](#requirement\_azurerm) | >= 3.100.0, < 5.0 |

## Modules

| Name | Source | Version |
|------|--------|---------|
| <a name="module_naming_convention"></a> [naming\_convention](#module\_naming\_convention) | pagopa-dx/azure-naming-convention/azurerm | ~> 0.0 |

## Resources

| Name | Type |
|------|------|
| [azurerm_linux_web_app.this](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/linux_web_app) | resource |
| [azurerm_linux_web_app_slot.this](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/linux_web_app_slot) | resource |
| [azurerm_private_endpoint.app_service_sites](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/private_endpoint) | resource |
| [azurerm_private_endpoint.staging_app_service_sites](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/private_endpoint) | resource |
| [azurerm_service_plan.this](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/service_plan) | resource |
| [azurerm_subnet.this](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/subnet) | resource |
| [azurerm_private_dns_zone.app_service](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/data-sources/private_dns_zone) | data source |
| [azurerm_virtual_network.this](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/data-sources/virtual_network) | data source |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_app_service_plan_id"></a> [app\_service\_plan\_id](#input\_app\_service\_plan\_id) | (Optional) Set the AppService Id where you want to host the Function App | `string` | `null` | no |
| <a name="input_app_settings"></a> [app\_settings](#input\_app\_settings) | Application settings | `map(string)` | n/a | yes |
| <a name="input_application_insights_connection_string"></a> [application\_insights\_connection\_string](#input\_application\_insights\_connection\_string) | (Optional) Application Insights connection string | `string` | `null` | no |
| <a name="input_application_insights_sampling_percentage"></a> [application\_insights\_sampling\_percentage](#input\_application\_insights\_sampling\_percentage) | (Optional) The sampling percentage of Application Insights. Default is 5 | `number` | `5` | no |
| <a name="input_environment"></a> [environment](#input\_environment) | Values which are used to generate resource names and location short names. They are all mandatory except for domain, which should not be used only in the case of a resource used by multiple domains. | <pre>object({<br/>    prefix          = string<br/>    env_short       = string<br/>    location        = string<br/>    domain          = optional(string)<br/>    app_name        = string<br/>    instance_number = string<br/>  })</pre> | n/a | yes |
| <a name="input_health_check_path"></a> [health\_check\_path](#input\_health\_check\_path) | Endpoint where health probe is exposed | `string` | n/a | yes |
| <a name="input_java_version"></a> [java\_version](#input\_java\_version) | Java version to use | `string` | `17` | no |
| <a name="input_node_version"></a> [node\_version](#input\_node\_version) | Node version to use | `number` | `20` | no |
| <a name="input_private_dns_zone_resource_group_name"></a> [private\_dns\_zone\_resource\_group\_name](#input\_private\_dns\_zone\_resource\_group\_name) | (Optional) The name of the resource group holding private DNS zone to use for private endpoints. Default is Virtual Network resource group | `string` | `null` | no |
| <a name="input_resource_group_name"></a> [resource\_group\_name](#input\_resource\_group\_name) | Resource group to deploy resources to | `string` | n/a | yes |
| <a name="input_slot_app_settings"></a> [slot\_app\_settings](#input\_slot\_app\_settings) | Staging slot application settings | `map(string)` | `{}` | no |
| <a name="input_stack"></a> [stack](#input\_stack) | n/a | `string` | `"node"` | no |
| <a name="input_sticky_app_setting_names"></a> [sticky\_app\_setting\_names](#input\_sticky\_app\_setting\_names) | (Optional) A list of application setting names that are not swapped between slots | `list(string)` | `[]` | no |
| <a name="input_subnet_cidr"></a> [subnet\_cidr](#input\_subnet\_cidr) | (Optional) CIDR block to use for the subnet the AppService uses for outbound connectivity. Mandatory if subnet\_id is not set | `string` | `null` | no |
| <a name="input_subnet_id"></a> [subnet\_id](#input\_subnet\_id) | (Optional) Set the subnet id where you want to host the Function App | `string` | `null` | no |
| <a name="input_subnet_pep_id"></a> [subnet\_pep\_id](#input\_subnet\_pep\_id) | Id of the subnet which holds private endpoints | `string` | n/a | yes |
| <a name="input_subnet_service_endpoints"></a> [subnet\_service\_endpoints](#input\_subnet\_service\_endpoints) | (Optional) Enable service endpoints for the underlying subnet. This variable should be set only if function dependencies do not use private endpoints | <pre>object({<br/>    cosmos  = optional(bool, false)<br/>    storage = optional(bool, false)<br/>    web     = optional(bool, false)<br/>  })</pre> | `null` | no |
| <a name="input_tags"></a> [tags](#input\_tags) | Resources tags | `map(any)` | n/a | yes |
| <a name="input_tier"></a> [tier](#input\_tier) | Resource tiers depending on workload. Allowed values are 's', 'm', 'l', 'xl'. Legacy values 'premium', 'standard', 'test' are also supported for backward compatibility. | `string` | `"l"` | no |
| <a name="input_virtual_network"></a> [virtual\_network](#input\_virtual\_network) | Virtual network in which to create the subnet | <pre>object({<br/>    name                = string<br/>    resource_group_name = string<br/>  })</pre> | n/a | yes |

## Outputs

| Name | Description |
|------|-------------|
| <a name="output_app_service"></a> [app\_service](#output\_app\_service) | n/a |
| <a name="output_subnet"></a> [subnet](#output\_subnet) | n/a |
<!-- END_TF_DOCS -->
