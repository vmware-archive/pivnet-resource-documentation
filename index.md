
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

## API_TOKEN
The `api-token` can be obtained at: 
![Token](https://s3.amazonaws.com/pivnet-resource-page/tokenSelection.png)

Navigate here by going to your personal profile on [Pivnet](https://network.pivotal.io). Note that you can use either the legacy token or the UAA Refresh token. Read more details about how the difference between the tokens and how they work [here](https://network.pivotal.io/docs/api#how-to-authenticate).

## PRODUCT_SLUG
The `product-slug` defines the product that you want to be fetching from [Pivnet](https://network.pivotal.io). You can see the product slug in the URL of each product.
![Slug](https://s3.amazonaws.com/pivnet-resource-page/pivnet-product-slug.png). 


## PRODUCT_VERSION
The `product-version` for each product is an optional field. If you do not specify it, it will pull down the latest release by default. To specify a specific product_version, select the release you wish to download from [Pivnet](https://network.pivotal.io) and specify the version. **Note:** This field takes a [regex](https://en.wikipedia.org/wiki/Regular_expression).

Select the version from the dropdown:
![Version](https://s3.amazonaws.com/pivnet-resource-page/pivnet-product-version.png)












# Header 1

This is a normal paragraph following a header. GitHub is a code hosting platform for version control and collaboration. It lets you and others work together on projects from anywhere.

## Header 2

> This is a blockquote following a header.
>
> When something is important enough, you do it even if the odds are not in your favor.

### Header 3

```js
// Javascript code with syntax highlighting.
var fun = function lang(l) {
  dateformat.i18n = require('./lang/' + l)
  return true;
}
```

```ruby
# Ruby code with syntax highlighting
GitHubPages::Dependencies.gems.each do |gem, version|
  s.add_dependency(gem, "= #{version}")
end
```

#### Header 4

*   This is an unordered list following a header.
*   This is an unordered list following a header.
*   This is an unordered list following a header.

##### Header 5

1.  This is an ordered list following a header.
2.  This is an ordered list following a header.
3.  This is an ordered list following a header.

###### Header 6

| head1        | head two          | three |
|:-------------|:------------------|:------|
| ok           | good swedish fish | nice  |
| out of stock | good and plenty   | nice  |
| ok           | good `oreos`      | hmm   |
| ok           | good `zoute` drop | yumm  |

### There's a horizontal rule below this.

* * *

### Here is an unordered list:

*   Item foo
*   Item bar
*   Item baz
*   Item zip

### And an ordered list:

1.  Item one
1.  Item two
1.  Item three
1.  Item four

### And a nested list:

- level 1 item
  - level 2 item
  - level 2 item
    - level 3 item
    - level 3 item
- level 1 item
  - level 2 item
  - level 2 item
  - level 2 item
- level 1 item
  - level 2 item
  - level 2 item
- level 1 item

### Small image

![Octocat](https://assets-cdn.github.com/images/icons/emoji/octocat.png)

### Large image

![Branching](https://guides.github.com/activities/hello-world/branching.png)


### Definition lists can be used with HTML syntax.

<dl>
<dt>Name</dt>
<dd>Godzilla</dd>
<dt>Born</dt>
<dd>1952</dd>
<dt>Birthplace</dt>
<dd>Japan</dd>
<dt>Color</dt>
<dd>Green</dd>
</dl>

```
Long, single-line code blocks should not wrap. They should horizontally scroll if they are too long. This line should be long enough to demonstrate this.
```

```
The final element.
```
