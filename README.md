# Notes

## Standardize deployment workflow

Providers define individual units of infrastructure, you can compose from different providers into reasulable modules.

Terraform's configuration language is declarative, meaning that it describes the desired **end-state ** of your infra.
- automatic dependency caluclation

### Steps to Deploy with Terraform

1. Scope - Identitfy the infrastructure for your project
2. Author - Write the configuration for your infrastructure
3. Initialize - Install the plugins Terraform needs to manage the infrastructure
4. Plan - Preview the changes Terraform will make to match your configuration
5. Apply - Make the planned changes

## Track your infrastructure

Terraform keeps track of infra state in a state file

## Azure Setup

**Login **

```
az login
```

**Create a service principle**

A Service Principle is an applicaiton within Entra ID with the authentication tokens Terraform needs to perform actions.

```
az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/<SubscriptionId>"
```

**Save Environment Variables**

```
export ARM_CLIENT_ID=""
export ARM_CLIENT_SECRET=""
export ARM_SUBSCRIPTION_ID=""
export ARM_TENANT_ID=""
```

## Terraform Code

### Terraform Block

The `terraform {}` block contains Terraform settings, including the required providers.
- For each prodive the `source` attribute defines an optional hostname, a namespace, and the provider type.
- By default, providers are installed from the Terraform Registry by default `hashicorp/azurerm` is shorthand for `registry.terraform.io/hashicorp/azurerm`

You can also define a `version` for each provider, although not required it is recommended. Without specifying the version, Terraform will use the latest version for that provider which could cause problems.


### Providers

The `provider` block configures the specified provider.

### Resource

Use `resource` blocks to define components of your infra. They have two strings before the block:
1. The resource type
2. The resource name

## Deploying

### Initializing Terraform config

```
terraform init
```

### Format and validate config

Format your terraform files so that they all have the same formatting. Running this command formats configs in the current directory.
```
terraform fmt
```

Validate your configuration to ensure syntax validity and internal consistency.
```
terraform validate
```

### Applying Config

Run `terraform apply`, this is show the execution plan and will prompt for approval before proceeding.

## Inspecting State

Terraform writes data into a file called `terraform.tfstate`. It contains the IDs and properties of the resources created by Terraform so it can manage them.

This state fle contains all of the data in your config, and could also contain sensitive values in plain text, do not share.
- consider storing state remotely for teams and larger projects

You can inspect the state by running `terraform show`

## Updating

`~` prefix means that Terraform will update the resource in-place
- in-place means Terraform will modify the existing resource directly without destroying and recreating it

## Destroying

`terraform destroy` terminates resources managed by your Terraform project.

`-` prefix means that the instance will be destroyed

## Variables

Variables are defined within `variables.tf`

Using cli to set variables

```
terraform apply -var "resource_group_name=myNewResourceGroupName"
```

Updating a resource group name is a destructive update that forces Terraform to recreate the resource and subsequently and child resources.

## Outputs

The outputs you specified after an apply can be queried using `terraform output`.

To define outputs create a `outputs.tf` file.

When you add an output to a previously applied config, you must re-run `terraform apply`

## Migrating State to HCP Terraform

1. Login to HCP Terraform
2. Configure cloud block in terraform block

Remote or Local execution

If using remote execution, have to create a service principle and update the HCP Terraform environment variables.