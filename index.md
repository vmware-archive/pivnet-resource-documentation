
# Hello!

This page is an easy to understand explanation of how to use the Pivnet Resource in your pipelines to install [Pivotal](https://network.pivotal.io/) products! We'll walk you through how to use, setup, and integrate your pipelines to pull down the latest releases. 


# Understanding Pivnet Resource

In your pipeline, you will need to add the following under `resource_types`:

```yaml
---
resource_types:
- name: pivnet
  type: docker-image
  source:
    repository: pivotalcf/pivnet-resource
    tag: latest-final
```

And the following snippet under `resources`:

```yaml
resources:
- name: product-to-fetch-from-pivnet
  type: pivnet
  source:
    api_token: {{api-token}}
    product_slug: {{product-slug}}
    product_version: {{product-version}} # optional
```


Each product you want to fetch from [Pivnet](https://network.pivotal.io) will have to be defined as its own resource. A resource has many attributes (which you can read about in more detail [here](https://github.com/pivotal-cf/pivnet-resource)), but the important ones to understand when fetching products are `api-token`, `product-slug`, and `product-version`. 

### API_TOKEN
The `api-token` can be obtained at: 

![Token](https://s3.amazonaws.com/pivnet-resource-page/tokenSelection.png)

Navigate here by going to your personal profile on [Pivnet](https://network.pivotal.io). Note that you can use either the legacy token or the UAA Refresh token. Read more details about how the difference between the tokens and how they work [here](https://network.pivotal.io/docs/api#how-to-authenticate).



### PRODUCT_SLUG
The `product-slug` defines the product that you want to be fetching from [Pivnet](https://network.pivotal.io). You can see the product slug in the URL of each product.

![Slug](https://s3.amazonaws.com/pivnet-resource-page/pivnet-product-slug.png). 



### PRODUCT_VERSION
The `product-version` for each product is an optional field. If you do not specify it, it will pull down the latest release by default. To specify a specific product_version, select the release you wish to download from [Pivnet](https://network.pivotal.io) and specify the version. **Note:** This field takes a [regex](https://en.wikipedia.org/wiki/Regular_expression).

Select the version from the dropdown:
![Version](https://s3.amazonaws.com/pivnet-resource-page/pivnet-product-version.png)



# Example

A simple example of downloading the `stemcells` product can be seen [here](https://github.com/pivotal-cf/pivnet-resource/blob/master/examples/get-aws-vsphere-stemcells.yml).


A slightly more complex example:

```yaml
resource_types:
- name: pivnet
  type: docker-image
  source:
    repository: pivotalcf/pivnet-resource
    tag: latest-final

resources:
- name: healthwatch-product
  type: pivnet
  source:
    api_token: ((pivnet_token))
    product_slug: p-healthwatch
    product_version: ((healthwatch_version))
    sort_by: semver


jobs:
- name: get-healthwatch-product
  plan:
  - get: healthwatch-product
    params:
      globs: ["p-healthwatch*.pivotal"]
    trigger: true
```






