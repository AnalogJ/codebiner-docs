# Terraform Thycotic Plugin

A Terraform plugin to retrieve secrets from Thycotic Secret Server.

## Features

- Retrieve Secrets/Credentials from Thycotic Secret Server and use them in Terraform projects
- Metadata can also be retrieved
- Works with Terraform v0.10.x+
- Integrates with Thycotic Secret Server on Prem & Cloud
- Supports Windows, Linux, macOS
- Professional Support
- Simple Setup

## Requirements

- [Terraform](https://www.terraform.io/downloads.html) 0.10.x
- [Thycotic Secret Server](https://thycotic.com/products/secret-server/) Cloud or On Prem


## Getting Started

- Download the latest release for your OS. (Mac, Windows & Linux available)
- Copy the binary to your Terraform user plugins directory (**You may need to create this directory**):
    - `~/.terraform.d/plugins` on macOS, Linux
    - `<APPLICATION DATA>\terraform.d\plugins` on Windows
- Use the provider in your terraform project (copy example code in `#Usage` section)
- Replace `server` and `token` with your own credentials. See `#Credentials` section for more information

## Usage

Open (or create) a Terraform project. Add the following lines to configure the provider and retrieve credential data.

```hcl

provider "thycotic" {
    ### required ###
    server = "thycotic.corp.companyname.com"
    token = "XXXXX...XXX:YY...YY"

    ### optional ###
    # domain = "corp.companyname.com" # optional
}


data "thycotic_secret" "myresourcename" {
    # you can retrieve secrets by path or id
    id = "12345"
    # path = "/Personal Folders/username/subfolder/example.com"
}

# this is just for example purposes
output "username_password_data" {
  value = "${data.thycotic_secret.myresourcename.data}"
}

output "individual_field_name_value" {
  value = "${lookup(data.thycotic_secret.myresourcename.data, "username")}"
}

# output "metadata" {
#   value = "${data.thycotic_secret.myresourcename.metadata}"
# }
```

## Configuration

### Provider

```hcl
provider "thycotic" {
    ### required ###
    server = "thycotic.corp.example.com"
    token = "XXXXX...XXX:YY...YY"

    ### optional ###
    # domain = "corp.example.com"
    # version = "~> 1.0" # see https://www.terraform.io/docs/configuration/providers.html#provider-versions
    # alias = "thycotic_prod" # see https://www.terraform.io/docs/configuration/providers.html#multiple-provider-instances
}
```

The `server`, `token` and `domain` configuration options can also be specified via environmental variables:

- **THYCOTIC_SERVER**
- **THYCOTIC_TOKEN**
- **THYCOTIC_DOMAIN**

In the interest of security, It is recommended that you specify the `token` via environmental variable (**THYCOTIC_TOKEN**).

In additional to the `server`, `token` and `domain` configuration options, this plugin supports the Terraform built-in
[`version` constraints](https://www.terraform.io/docs/configuration/providers.html#provider-versions) and [`alias` system.](https://www.terraform.io/docs/configuration/providers.html#multiple-provider-instances)

### Data Source

```hcl
data "thycotic_secret" "example_com_credentials" {
    # `id` or `path` is required
    id = "12345"
    # path = "/Personal Folders/username/subfolder/example.com"

    ### optional ###
    # if you set a alias for the provider, you must reference it in the data source. See https://www.terraform.io/docs/configuration/providers.html#multiple-provider-instances
    # provider = "thycotic.thycotic_prod"
}
```

The Terraform plugin for Thycotic Secret Server supports retrieving secrets via 2 methods, `id` and `path`.

The secret `id` can be retrieved by viewing a secret and taking note of the `secretid` in the url:
`https://thycotic.corp.example.com/SecretView.aspx?secretid=12734`

The secret `path` can be determined by viewing a secret and joining the `Folder` and `Secret Name`.
All backslashes must be replaced by forward slashes.

```
Folder: \Personal Folders\username\subfolder
Secret Name: example.com

path: /Personal Folders/username/subfolder/example.com
```

### Credentials

A Thycotic Secret Server `token` can be retrieved by making an API request with your username and password.

```
curl \
-H "Content-Type: application/x-www-form-urlencoded" \
-d 'username=myusername@corp.example.com&password=mypassword&organization=&domain=' \
--url "https://corp.example.com/webservices/sswebservice.asmx/Authenticate"
```

The server will respond with an XML document with the following format:

```
<?xml version="1.0" encoding="utf-8"?>
<AuthenticateResult xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="urn:thesecretserver.com">
  <Errors />
  <Token>AgIClfYD5sUsuxJ0AhYlUqQO1PaFPqcbhIkDLoUIh7u7a4QuCRe82...</Token>
</AuthenticateResult>
```

Your personal token will be in the `<Token>` element. Treat it like a password.

This plugin also works with [Thycotic Secret Server Cloud](https://thycotic.com/products/secret-server/secret-server-online/) accounts, which are free for up to 250 secrets.

## Versioning
We use SemVer for versioning.

## Updates
Once new versions are available, you'll be notified via email.

## Author
- Jason Kulatunga

## License
All Rights Reserved

