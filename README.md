# Flexible Engine Peering Terraform Module

Terraform module which creates peering between VPC on Flexible Engine.

## Usage : Terraform

This module requires that many providers are created in terraform environment
description.

In case VPC are in the same tenant, don't forget to set **same_tenant** to true in module calling.

Example with 2 providers:

```hcl
provider "flexibleengine" {
  alias     = "tenant_stage"
  user_id   = "xxx"
  password  = "foo_bar"
  tenant_id = var.tenant_id_tenant_stage
  auth_url  = var.auth_url
  region    = var.region
}

provider "flexibleengine" {
  alias     = "tenant_prod"
  user_id   = "xxx"
  password  = "foo_bar"
  tenant_id = var.tenant_id_tenant_prod
  auth_url  = var.auth_url
  region    = var.region
}
```

Example of module call:

```hcl
module "peering_stage_prod" {
  source = "terraform-flexibleengine-modules/vpcpeering/flexibleengine"
  version = "1.0.0"

  providers = {
    flexibleengine.requester = flexibleengine.tenant_prod
    flexibleengine.accepter = flexibleengine.tenant_stage
  }

  peer_name = "peering-stage-prod"

  vpc_req_name       = "vpc-prod"

  vpc_acc_name       = "vpc-stage"

  req_subnet_cidr = [
    "10.10.1.0/24",
    "10.10.2.0/24",
  ]

  acc_subnet_cidr = [
    "192.168.1.0/24",
  ]

  tenant_acc_id = "TENANT_ACC_ID"
}
```

## Usage : Terragrunt

Example of module call:

```hcl
################################
### Terragrunt Configuration ###
################################

terraform {
  source = "terraform-flexibleengine-modules/vpcpeering/flexibleengine"
  version = "1.0.0"
}

include {
  path = find_in_parent_folders()
}

##################
### Parameters ###
##################

inputs = {

  
  providers = {
    flexibleengine.requester = flexibleengine.tenant_prod
    flexibleengine.accepter = flexibleengine.tenant_stage
  }

  peer_name = "peering-stage-prod"

  vpc_req_name       = "vpc-prod"

  vpc_acc_name       = "vpc-stage"

  req_subnet_cidr = [
    "10.10.1.0/24",
    "10.10.2.0/24",
  ]

  acc_subnet_cidr = [
    "192.168.1.0/24",
  ]

  tenant_acc_id = "TENANT_ACC_ID"
}
```
## Requirements

| Name | Version |
|------|---------|
| <a name="requirement_terraform"></a> [terraform](#requirement\_terraform) | >= 0.13 |

## Providers

| Name | Version |
|------|---------|
| <a name="provider_flexibleengine.accepter"></a> [flexibleengine.accepter](#provider\_flexibleengine.accepter) | n/a |
| <a name="provider_flexibleengine.requester"></a> [flexibleengine.requester](#provider\_flexibleengine.requester) | n/a |

## Modules

No modules.

## Resources

| Name | Type |
|------|------|
| [flexibleengine_vpc_peering_connection_accepter_v2.peer](https://registry.terraform.io/providers/FlexibleEngineCloud/flexibleengine/latest/docs/resources/vpc_peering_connection_accepter_v2) | resource |
| [flexibleengine_vpc_peering_connection_v2.peering](https://registry.terraform.io/providers/FlexibleEngineCloud/flexibleengine/latest/docs/resources/vpc_peering_connection_v2) | resource |
| [flexibleengine_vpc_route_v2.acc_vpc_route](https://registry.terraform.io/providers/FlexibleEngineCloud/flexibleengine/latest/docs/resources/vpc_route_v2) | resource |
| [flexibleengine_vpc_route_v2.req_vpc_route](https://registry.terraform.io/providers/FlexibleEngineCloud/flexibleengine/latest/docs/resources/vpc_route_v2) | resource |
| [flexibleengine_vpc_v1.vpc_acc](https://registry.terraform.io/providers/FlexibleEngineCloud/flexibleengine/latest/docs/data-sources/vpc_v1) | data source |
| [flexibleengine_vpc_v1.vpc_req](https://registry.terraform.io/providers/FlexibleEngineCloud/flexibleengine/latest/docs/data-sources/vpc_v1) | data source |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_acc_subnet_cidr"></a> [acc\_subnet\_cidr](#input\_acc\_subnet\_cidr) | list of accepter's subnet CIDR | `list` | `[]` | no |
| <a name="input_peer_name"></a> [peer\_name](#input\_peer\_name) | Name of the peering connection | `string` | `""` | no |
| <a name="input_req_subnet_cidr"></a> [req\_subnet\_cidr](#input\_req\_subnet\_cidr) | list of requester's subnet CIDR | `list` | `[]` | no |
| <a name="input_same_tenant"></a> [same\_tenant](#input\_same\_tenant) | Indicates VPC are in the same tenant | `bool` | `false` | no |
| <a name="input_tenant_acc_id"></a> [tenant\_acc\_id](#input\_tenant\_acc\_id) | tenant ID of the accepter | `string` | `""` | no |
| <a name="input_vpc_acc_id"></a> [vpc\_acc\_id](#input\_vpc\_acc\_id) | ID of accepter's VPC | `any` | `null` | no |
| <a name="input_vpc_acc_name"></a> [vpc\_acc\_name](#input\_vpc\_acc\_name) | Name of accepter's VPC | `any` | `null` | no |
| <a name="input_vpc_req_id"></a> [vpc\_req\_id](#input\_vpc\_req\_id) | ID of the requester's VPC | `any` | `null` | no |
| <a name="input_vpc_req_name"></a> [vpc\_req\_name](#input\_vpc\_req\_name) | Name of the requester's VPC | `any` | `null` | no |

## Outputs

| Name | Description |
|------|-------------|
| <a name="output_peering_id"></a> [peering\_id](#output\_peering\_id) | ID of the created peering |
