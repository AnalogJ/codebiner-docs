# Terraform Conjur Plugin

A Terraform plugin to retrieve secrets from Cyberark Conjur.

## Features

- Retrieve Secrets/Credentials from Cyberark Conjur and use them in Terraform projects
- Metadata can also be retrieved
- Works with Terraform v0.10.x+
- Integrates with Cyberark Conjur
- Supports Windows, Linux, macOS
- Professional Support
- Simple Setup

## Requirements

- [Terraform](https://www.terraform.io/downloads.html) 0.10.x
- [Cyberark Conjur](https://www.conjur.org/)


## Getting Started

- Download the latest release for your OS. (Mac, Windows & Linux available)
- Copy the binary to your Terraform user plugins directory (**You may need to create this directory**):
    - `~/.terraform.d/plugins` on macOS, Linux
    - `<APPLICATION DATA>\terraform.d\plugins` on Windows
- Use the provider in your terraform project (copy example code in `#Usage` section)
- Replace `login`, `api_key`, `appliance_url` and `account`  with your own credentials. See `#Credentials` section for more information

## Usage

Open (or create) a Terraform project. Add the following lines to configure the provider and retrieve credential data.

```hcl

provider "conjur" {
    ### required ###
    login = "username"
    api_key = "xxxxxx"
    appliance_url = "https://conjur.corp.example.com"
    account = "myorg"
}


data "conjur_secret" "myresourcename" {
    # you can retrieve secrets by variableid
    id = "eval/secret"
}

# this is just for example purposes
output "credential_data_value" {
  value = "${lookup(data.conjur_secret.myresourcename.data, "text")}"
}

# output "metadata" {
#   value = "${data.conjur_secret.myresourcename.metadata}"
# }
```

## Configuration

### Provider

```hcl
provider "conjur" {
    ### required ###
    login = "username"
    api_key = "xxxxxx"
    appliance_url = "https://conjur.corp.example.com"
    account = "myorg"

    ### optional ###
    # version = "~> 1.0" # see https://www.terraform.io/docs/configuration/providers.html#provider-versions
    # alias = "conjur_prod" # see https://www.terraform.io/docs/configuration/providers.html#multiple-provider-instances
}
```

The `login`, `api_key`, `appliance_url` and `account` configuration options can also be specified via environmental variables:

- **CONJUR_LOGIN**
- **CONJUR_API_KEY**
- **CONJUR_APPLIANCE_URL**
- **CONJUR_ACCOUNT**

In the interest of security, It is recommended that you specify the `api_key` via environmental variable (**CONJUR_API_KEY**).

In additional to the  `login`, `api_key`, `appliance_url` and `account` configuration options, this plugin supports the Terraform built-in
[`version` constraints](https://www.terraform.io/docs/configuration/providers.html#provider-versions) and [`alias` system.](https://www.terraform.io/docs/configuration/providers.html#multiple-provider-instances)

### Data Source

```hcl
data "conjur_secret" "example_com_credentials" {
    # you can retrieve secrets by variableid
    id = "eval/secret"

    ### optional ###
    # if you set a alias for the provider, you must reference it in the data source. See https://www.terraform.io/docs/configuration/providers.html#multiple-provider-instances
    # provider = "conjur.conjur_prod"
}
```

The Terraform plugin for Cyberark Conjur supports retrieving secrets via policy varible id's.


### Credentials

A Conjur entity (either host or user) has it's own unique API key which is used along with the id for authentication.

An API key is an alphanumeric string with length of 51 to 56 characters. It is generated randomly during identity creation, and can be rotated at a later time.

This plugin also works with [Cyberark Conjur Eval Server](https://www.conjur.org/get-started/try-conjur.html) accounts,
which is free but is public and should only be used for evaluation purposes.

## Versioning
We use SemVer for versioning.

## Updates
Once new versions are available, you'll be notified via email.

## Author
- Jason Kulatunga

## License
All Rights Reserved
