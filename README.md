<!-- BEGIN_TF_DOCS -->
# Bastion

This module simulate a module would be created by the Support/Testing team for an architecture.
In this module, we will use to create a Bastion.

This is a very easy architecture but we will try to simulate real time IT team working and collaboration with good practice.

* I created this module separatly and at last to differenciate from other and I will be easy to manage beause in some cases we don't need bastion.

```hcl
resource "azurerm_subnet" "bastion" {
  name                 = "AzureBastionSubnet"
  resource_group_name  = var.rg_name
  virtual_network_name = var.vnet_name
  address_prefixes     = [var.bastion_subnet_cidr]
}
resource "azurerm_public_ip" "bastion" {
  name                = var.bastion_ip_name
  location            = var.location
  resource_group_name = var.rg_name
  allocation_method   = "Static"
  sku                 = "Standard"
  tags                = var.tags
}
resource "azurerm_bastion_host" "bastion" {
  name                = var.bastion_name
  location            = azurerm_public_ip.bastion.location
  resource_group_name = azurerm_public_ip.bastion.resource_group_name
  sku                 = "Standard"

  ip_configuration {
    name                 = "bastion-config"
    subnet_id            = azurerm_subnet.bastion.id
    public_ip_address_id = azurerm_public_ip.bastion.id
  }
  tags = var.tags
}

# pour faire fonctionner bastion en locals
# az network bastion tunnel \
#   --name bastion-host \
#   --resource-group my-rg \
#   --target-resource-id $(az vm show -g my-rg -n vm1 --query id -o tsv) \
#   --resource-port 22 \
#   --port 50022
```

<!-- markdownlint-disable MD033 -->
## Requirements

No requirements.

## Providers

The following providers are used by this module:

- <a name="provider_azurerm"></a> [azurerm](#provider\_azurerm)

## Resources

The following resources are used by this module:

- [azurerm_bastion_host.bastion](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/bastion_host) (resource)
- [azurerm_public_ip.bastion](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/public_ip) (resource)
- [azurerm_subnet.bastion](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/subnet) (resource)

<!-- markdownlint-disable MD013 -->
## Required Inputs

The following input variables are required:

### <a name="input_bastion_ip_name"></a> [bastion\_ip\_name](#input\_bastion\_ip\_name)

Description: n/a

Type: `string`

### <a name="input_bastion_name"></a> [bastion\_name](#input\_bastion\_name)

Description: n/a

Type: `string`

### <a name="input_bastion_subnet_cidr"></a> [bastion\_subnet\_cidr](#input\_bastion\_subnet\_cidr)

Description: CIDR fixe pour le subnet Bastion (doit être /26 min)

Type: `string`

### <a name="input_location"></a> [location](#input\_location)

Description: n/a

Type: `string`

### <a name="input_rg_name"></a> [rg\_name](#input\_rg\_name)

Description: n/a

Type: `string`

### <a name="input_vnet_name"></a> [vnet\_name](#input\_vnet\_name)

Description: n/a

Type: `string`

## Optional Inputs

The following input variables are optional (have default values):

### <a name="input_tags"></a> [tags](#input\_tags)

Description: n/a

Type: `map(string)`

Default:

```json
{
  "module": "bastion for ssh"
}
```

## Outputs

The following outputs are exported:

### <a name="output_bastion_details"></a> [bastion\_details](#output\_bastion\_details)

Description: n/a

## Modules

No modules.

This module created by paul for eductional and preparation for Terraform associate 003 purpous.
<!-- END_TF_DOCS -->