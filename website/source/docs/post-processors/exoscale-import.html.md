---
description: |
    The Packer Exoscale Import post-processor takes an image artifact
    from various builders and imports it to Exoscale.
layout: docs
page_title: 'Exoscale Import - Post-Processors'
sidebar_current: 'docs-post-processors-exoscale-import'
---

# Exoscale Import Post-Processor

Type: `exoscale-import`

The Packer Exoscale Import post-processor takes an image artifact from
the QEMU, Artifice, or File builders and imports it to Exoscale.

## How Does it Work?

The import process operates uploading a temporary copy of the image to
Exoscale's [Object Storage](https://www.exoscale.com/object-storage/) (SOS)
and then importing it as a Private Template via the Exoscale API. The
temporary copy in SOS can be discarded after the import is complete.

For more information about Exoscale Private Templates, see the
[documentation](https://community.exoscale.com/documentation/compute/private-templates/).

## Configuration

There are some configuration options available for the post-processor.

Required:

-   `api_key` (string) - The API key used to communicate with Exoscale
    services. This may also be set using the `EXOSCALE_API_KEY` environmental
    variable.

-   `api_secret` (string) - The API secret used to communicate with Exoscale
    services. This may also be set using the `EXOSCALE_API_SECRET`
    environmental variable.

-   `image_bucket` (string) - The name of the bucket in which to upload the
    template image to SOS. The bucket must exist when the post-processor is
    run.

-   `template_name` (string) - The name to be used for registering the template.

-   `template_description` (string) - The description for the registered template.

Optional:

-   `api_endpoint` (string) - The API endpoint used to communicate with the
    Exoscale API. Defaults to `https://api.exoscale.com/compute`.

-   `sos_endpoint` (string) - The endpoint used to communicate with SOS.
    Defaults to `https://sos-ch-gva-2.exo.io`.

-   `template_zone` (string) - The Exoscale [zone](https://www.exoscale.com/datacenters/)
    in which to register the template. Defaults to `ch-gva-2`.

-   `template_username` (string) - An optional username to be used to log into
    Compute instances using this template.

-   `template_disable_password` (boolean) - Whether the registered template
    should disable Compute instance password reset. Defaults to `false`.

-   `template_disable_sshkey` (boolean) - Whether the registered template
    should disable SSH key installation during Compute instance creation.
    Defaults to `false`.

-   `skip_clean` (boolean) - Whether we should skip removing the image file
    uploaded to SOS after the import process has completed. "true" means that
    we should leave it in the bucket, "false" means deleting it.
    Defaults to `false`.

## Basic Example

Here is a basic example:

``` json
{
  "type": "exoscale-import",
  "api_key": "{{user `exoscale_api_key`}}",
  "api_secret": "{{user `exoscale_api_secret`}}",
  "image_bucket": "my-templates",
  "template_name": "myapp",
  "template_description": "myapp v1.2.3",
  "template_username": "admin"
}
```
