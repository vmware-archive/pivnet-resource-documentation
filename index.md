# Pivnet Resource

The [Pivnet Resource](https://github.com/pivotal-cf/pivnet-resource) can be used in your [Concourse](https://concourse-ci.org/) pipeline to download product releases from [Pivotal Network](https://network.pivotal.io/). This page will walk you through how to use, setup, and integrate the Pivnet Resource in your Concourse pipeline.

Before you begin, you will need the following:

1. A [Concourse](https://concourse-ci.org/) pipeline
2. Access to the Internet
3. A [Pivotal Network](https://network.pivotal.io) account


# Get Started with Pivnet Resource

The first thing you need to do is define the resource type. In your pipeline, you will need to add the following snippet under `resource_types`:

```yaml
---
resource_types:
- name: pivnet
  type: docker-image
  source:
    repository: pivotalcf/pivnet-resource
    tag: latest-final
```


Each product you want to fetch from [Pivotal Network](https://network.pivotal.io) will have to be defined as its own resource (in the example below, we are pulling two products). Pivnet Resource has many [attributes](https://github.com/pivotal-cf/pivnet-resource)), but the important ones to understand when fetching products are [api-token](#api_token), [product-slug](#product_slug), and [product-version](#product_version). 


```yaml
resources:
- name: mysql-product
  type: pivnet
  source:
    api_token: ((my-token))
    product_slug: ((p-mysql))
    product_version: ((1\.2\.3)) # optional

- name: cloudcache-product
  type: pivnet
  source:
    api_token: my-token
    product_slug: p-cloudcache 
    # no product_version is defined, so the latest release will be pulled
```



### API_TOKEN
To get your `api-token`, navigate to your personal profile on [Pivotal Network](https://network.pivotal.io). Note that you can use either the legacy token or the UAA refresh token. Read more details about the difference between the tokens and how they work [here](https://network.pivotal.io/docs/api#how-to-authenticate). Your token profile selection screen will look similar to:

![Token](https://raw.githubusercontent.com/pivotal-cf/pivnet-resource-page.github.io/master/pivnet-profile.png)



### PRODUCT_SLUG
The `product-slug` defines the product that you want to be fetching from [Pivotal Network](https://network.pivotal.io). You can see the product slug in the URL of each product.

![Slug](https://raw.githubusercontent.com/pivotal-cf/pivnet-resource-page.github.io/master/pivnet-product-slug.png). 



### PRODUCT_VERSION
The `product-version` refers to a release of a product. It is an optional field to specify on the resource. If you do not specify it, it will pull down the latest release  by default. To specify a specific `product_version`, select the release you wish to download from [Pivotal Network](https://network.pivotal.io) and specify the version. **Note:** This field takes a [regex](https://en.wikipedia.org/wiki/Regular_expression).

Select the version from the dropdown:
![Version](https://raw.githubusercontent.com/pivotal-cf/pivnet-resource-page.github.io/master/pivnet-product-version.png)


# Example

A simple example of downloading the [healthwatch](https://network.pivotal.io/products/p-healthwatch/) and [cloudcache](https://network.pivotal.io/products/p-cloudcache/) products can be seen below:

```yaml
---
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
    api_token: ((api_token))
    product_slug: p-healthwatch
    product_version: 1\.2\.2

- name: cloudcache-product
  type: pivnet
  source:
    api_token: ((api_token))
    product_slug: p-cloudcache
    # no product_version is specified, so the latest release will be fetched

- name: repo
  type: git
  source:
    uri: git@github.com:pivotal-cf/repo.git
    branch: master

jobs:
- name: get-products
  plan:
  - get: healthwatch-product
  - get: cloudcache-product
  - task: output-name
    file: repo/ci/tasks/fetch.yml
```


# Demo






