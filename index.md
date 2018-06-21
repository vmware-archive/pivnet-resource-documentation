# Pivnet Resource

The [Pivnet Resource](https://github.com/pivotal-cf/pivnet-resource) can be used in your [concourse](https://concourse-ci.org/) pipeline to download product releases from [Pivotal Network](https://network.pivotal.io/). Before you begin, you will need the following:

1. A [concourse](https://concourse-ci.org/) pipeline
2. Access to the Internet
3. A [Pivotal Network](https://network.pivotal.io) account

This page will walk you through how to use, setup, and integrate the Pivnet Resource in your Concourse pipeline.


# Understanding the Resource

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

This defines the pivnet resource type you will use to pull down products from Pivotal Network.


And the following snippet under `resources`:

```yaml
resources:
- name: mysql-product
  type: pivnet
  source:
    api_token: my-token
    product_slug: p-mysql
    product_version: 1\.2\.3 # optional

- name: cloudcache-product
  type: pivnet
  source:
    api_token: my-token
    product_slug: p-cloudcache 
    # no product_version is defined, so the latest release will be pulled
```

Each product you want to fetch from [Pivotal Network](https://network.pivotal.io) will have to be defined as its own resource (in the example above, we are pulling two products). A resource has many attributes (which you can read about in more detail [here](https://github.com/pivotal-cf/pivnet-resource)), but the important ones to understand when fetching products are `api-token`, `product-slug`, and `product-version`. 

### API_TOKEN
To get your `api-token`, navigate to your personal profile on [Pivotal Network](https://network.pivotal.io). Note that you can use either the legacy token or the UAA refresh token. Read more details about how the difference between the tokens and how they work [here](https://network.pivotal.io/docs/api#how-to-authenticate). Your token profile selection screen will look similar to:

![Token](https://s3.amazonaws.com/pivnet-resource-page/tokenSelection.png)



### PRODUCT_SLUG
The `product-slug` defines the product that you want to be fetching from [Pivnet](https://network.pivotal.io). You can see the product slug in the URL of each product.

![Slug](https://s3.amazonaws.com/pivnet-resource-page/pivnet-product-slug.png). 



### PRODUCT_VERSION
The `product-version` refers to a release of a product. It is an optional field to specify on the resource. If you do not specify it, it will pull down the latest release  by default. To specify a specific product_version, select the release you wish to download from [Pivotal Network](https://network.pivotal.io) and specify the version. **Note:** This field takes a [regex](https://en.wikipedia.org/wiki/Regular_expression).

Select the version from the dropdown:
![Version](https://s3.amazonaws.com/pivnet-resource-page/pivnet-product-version.png)


# Example

A simple example of downloading the [healthwatch](https://network.pivotal.io/products/p-healthwatch/) product can be seen below:

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
    api_token: api-token
    product_slug: p-healthwatch
    product_version: 1\.1\.1

jobs:
- name: get-healthwatch-product
  plan:
  - get: healthwatch-product
    trigger: true
    params:
      globs: ["p-healthwatch*.pivotal"]
```


**Note:** The `globs` section refers to filenames in a release. Read more about the different parameters [here](https://github.com/pivotal-cf/pivnet-resource).








