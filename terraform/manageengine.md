# Terraform Manage Engine Plugin

A Terraform plugin to retrieve secrets from Manage Engine Password Manager Pro.

## Features

- Retrieve Secrets/Credentials from Manage Engine Password Manager Pro and use them in Terraform projects
- Metadata can also be retrieved
- Works with Terraform v0.10.x+
- Integrates with Manage Engine Password Manager Pro
- Supports Windows, Linux, macOS
- Professional Support
- Simple Setup

## Requirements

- [Terraform](https://www.terraform.io/downloads.html) 0.10.x
- [Manage Engine Password Manager Pro](https://www.manageengine.com/products/passwordmanagerpro/?MEtab)


## Getting Started

- Download the latest release for your OS. (Mac, Windows & Linux available)
- Copy the binary to your Terraform user plugins directory (**You may need to create this directory**):
    - `~/.terraform.d/plugins` on macOS, Linux
    - `<APPLICATION DATA>\terraform.d\plugins` on Windows
- Use the provider in your terraform project (copy example code in `#Usage` section)
- Replace `host` and `token`  with your own credentials. See `#Credentials` section for more information

## Usage

Open (or create) a Terraform project. Add the following lines to configure the provider and retrieve credential data.

```hcl

provider "manageengine" {
    ### required ###
    host = "passwordmanagerpro.corp.example.com"
    token = "XXXX-XXXX-XXXX-XXXX"
}


data "manageengine_secret" "myresourcename" {
    # you can retrieve secrets by resource id and account id
   resourceid = "913",
   id = "989"

   # or you can retrieve them using resource name and account name.
   path = "resource_name/account_name"
}

# this is just for example purposes
output "username_password_data" {
  value = "${data.manageengine_secret.myresourcename.data}"
}

output "individual_field_name_value" {
  value = "${lookup(data.manageengine_secret.myresourcename.data, "username")}"
}

# output "metadata" {
#   value = "${data.manageengine_secret.myresourcename.metadata}"
# }
```

## Configuration

### Provider

```hcl
provider "manageengine" {
    ### required ###
    host = "passwordmanagerpro.corp.example.com"
    token = "XXXX-XXXX-XXXX-XXXX"

    ### optional ###
    # scheme = "http" # or "https"
    # version = "~> 1.0" # see https://www.terraform.io/docs/configuration/providers.html#provider-versions
    # alias = "manageengine_prod" # see https://www.terraform.io/docs/configuration/providers.html#multiple-provider-instances
}
```

The `host`, `token` and `scheme` configuration options can also be specified via environmental variables:

- **MANAGEENGINE_HOST**
- **MANAGEENGINE_TOKEN**
- **MANAGEENGINE_SCHEME**

In the interest of security, It is recommended that you specify the `token` via environmental variable (**MANAGEENGINE_TOKEN**).

In additional to the  `host`, `token` and `scheme` configuration options, this plugin supports the Terraform built-in
[`version` constraints](https://www.terraform.io/docs/configuration/providers.html#provider-versions) and [`alias` system.](https://www.terraform.io/docs/configuration/providers.html#multiple-provider-instances)

### Data Source

```hcl
data "manageengine_secret" "server_root_user" {
    # you can retrieve secrets by account id and resource id
    resourceid = "913",
    id = "989"

    #or by resource name and account name
    path = "databaseserver/root"

    ### optional ###
    # if you set a alias for the provider, you must reference it in the data source. See https://www.terraform.io/docs/configuration/providers.html#multiple-provider-instances
    # provider = "manageengine.manageengine_prod"
}
```

The Terraform plugin for Manage Engine Password Manager Pro supports retrieving secrets via either resource & account ids
or the more descriptive resource & account names.


### Credentials

An API user must be created before you can use the terraform plugin for Manage Engine.
Follow the instructions here: [Create API user accounts in Password Manager Pro
](https://www.manageengine.com/products/passwordmanagerpro/help/restapi.html)

An API token is an alphanumeric string. It is generated randomly during API user creation, and can be rotated at a later time.

## Versioning
We use SemVer for versioning.

## Updates
Once new versions are available, you'll be notified via email.

## Author
- Jason Kulatunga

## License
All Rights Reserved
