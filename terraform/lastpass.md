# Terraform Lastpass Plugin

A Terraform plugin to retrieve secrets from Lastpass.

## Features

- Retrieve Secrets/Credentials from Lastpass and use them in Terraform projects
- Metadata can also be retrieved
- Works with Terraform v0.10.x+
- Integrates with Lastpass
- Supports Windows, Linux, macOS
- Professional Support
- Simple Setup

## Requirements

- [Terraform](https://www.terraform.io/downloads.html) 0.10.x
- [Lastpass](https://lastpass.com/) Account


## Getting Started

- Download the latest release for your OS. (Mac, Windows & Linux available)
- Copy the binary to your Terraform user plugins directory (**You may need to create this directory**):
    - `~/.terraform.d/plugins` on macOS, Linux
    - `<APPLICATION DATA>\terraform.d\plugins` on Windows
- Use the provider in your terraform project (copy example code in `#Usage` section)
- Replace `username` and `password` with your own credentials. See `#Credentials` section for more information

## Usage

Open (or create) a Terraform project. Add the following lines to configure the provider and retrieve credential data.

```hcl

provider "lastpass" {
    ### required ###
    username = "username@email.com"
    password = "xxxxxx"
}


data "lastpass_secret" "myresourcename" {
    # you can retrieve secrets by id
    id = "717787988672234323"
}

# this is just for example purposes
output "username_password_data" {
  value = "${data.lastpass_secret.myresourcename.data}"
}

output "individual_field_name_value" {
  value = "${lookup(data.lastpass_secret.myresourcename.data, "username")}"
}

# output "metadata" {
#   value = "${data.lastpass_secret.myresourcename.metadata}"
# }
```

## Configuration

### Provider

```hcl
provider "lastpass" {
    ### required ###
    username = "user@email.com"
    password = "xxxxxx"

    ### optional ###
    # version = "~> 1.0" # see https://www.terraform.io/docs/configuration/providers.html#provider-versions
    # alias = "lastpass_prod" # see https://www.terraform.io/docs/configuration/providers.html#multiple-provider-instances
}
```

The `username` and `password` configuration options can also be specified via environmental variables:

- **LASTPASS_USERNAME**
- **LASTPASS_PASSWORD**

In the interest of security, It is recommended that you specify the `password` via environmental variable (**LASTPASS_PASSWORD**).

In additional to the `username` and `password` configuration options, this plugin supports the Terraform built-in
[`version` constraints](https://www.terraform.io/docs/configuration/providers.html#provider-versions) and [`alias` system.](https://www.terraform.io/docs/configuration/providers.html#multiple-provider-instances)

### Data Source

```hcl
data "lastpass_secret" "example_com_credentials" {
    # you can retrieve secrets by id
    id = "717787988672234323"

    ### optional ###
    # if you set a alias for the provider, you must reference it in the data source. See https://www.terraform.io/docs/configuration/providers.html#multiple-provider-instances
    # provider = "lastpass.lastpass_prod"
}
```

The Terraform plugin for Lastpass supports retrieving secrets via id.

The secret `id` can be retrieved by using a tool like [tentacle](https://github.com/AnalogJ/tentacle) and taking note of the `id` after listing your secrets.

### Credentials

Lastpass accounts are free, you can sign up for one here: [Lastpass](https://www.lastpass.com/)

## Versioning
We use SemVer for versioning.

## Updates
Once new versions are available, you'll be notified via email.

## Author
- Jason Kulatunga

## License
All Rights Reserved

