---
title: Get started with Ruby - Strapi Developer Docs
description: Build powerful applications using Strapi, the leading open-source headless cms and Ruby.
canonicalUrl: https://docs.strapi.io/developer-docs/latest/developer-resources/content-api/integrations/ruby.html
---

# Getting Started with Ruby

!!!include(developer-docs/latest/developer-resources/content-api/snippets/integration-guide-not-updated.md)!!!

This integration guide is following the [Quick Start Guide](/developer-docs/latest/getting-started/quick-start.md). We assume that you have fully completed its "Hands-on" path, and therefore can consume the API by browsing this [url](http://localhost:1337/api/restaurants).

If you haven't gone through the Quick Start Guide, the way you request a Strapi API with [Ruby](https://www.ruby-lang.org/en/) remains the same except that you will not fetch the same content.

## Create a Ruby file

Be sure to have [Ruby installed](https://www.ruby-lang.org/en/documentation/installation/) on your computer.

```bash
mkdir ruby-app && cd ruby-app
touch script.rb
```

## Use an HTTP client

Many HTTP clients are available but in this documentation we'll use [HTTParty](https://github.com/jnunemaker/httparty).

- Create a `Gemfile` containing the following:

```
source "https://rubygems.org"

gem "httparty"
```

- Install your gems by running the following command:

```bash
bundle install
```

## GET Request your collection type

Execute a `GET` request on the `restaurant` Collection Type in order to fetch all your restaurants.

Be sure that you activated the `find` permission for the `restaurant` Collection Type.

:::: api-call
::: request Example GET request

```ruby
HTTParty.get('http://localhost:1337/api/restaurants',
  header: {
    'Content-Type' => 'application/json'
  })
```

:::

::: response Example response

```json
[
  {
    "id": 1,
    "name": "Biscotte Restaurant",
    "description": "Welcome to Biscotte restaurant! Restaurant Biscotte offers a cuisine based on fresh, quality products, often local, organic when possible, and always produced by passionate producers.",
    "created_by": {
      "id": 1,
      "firstname": "Paul",
      "lastname": "Bocuse",
      "username": null
    },
    "updated_by": {
      "id": 1,
      "firstname": "Paul",
      "lastname": "Bocuse",
      "username": null
    },
    "created_at": "2020-07-31T11:37:16.964Z",
    "updated_at": "2020-07-31T11:37:16.975Z",
    "categories": [
      {
        "id": 1,
        "name": "French Food",
        "created_by": 1,
        "updated_by": 1,
        "created_at": "2020-07-31T11:36:23.164Z",
        "updated_at": "2020-07-31T11:36:23.172Z"
      }
    ]
  }
]
```
:::
::::

### Example

```ruby
require 'httparty'

class Restaurant
  include HTTParty

  def initialize
    @api_url = "http://localhost:1337"
  end

  def all
    self.class.get("#{@api_url}/restaurants")
  end

end

restaurant = Restaurant.new
puts restaurant.all
```

## POST Request your collection type

Execute a `POST` request on the `restaurant` Collection Type in order to create a restaurant.

Be sure that you activated the `create` permission for the `restaurant` Collection Type and the `find` permission for the `category` Collection type.

In this example a `japanese` category has been created which has the id: 3.

:::: api-call
::: request Example POST request

```ruby
HTTParty.post("http://localhost:1337/api/restaurants",
  body: {
    name: 'Dolemon Sushi',
    description: 'Unmissable Japanese Sushi restaurant. The cheese and salmon makis are delicious',
    categories: [3],
  },
  header: {
    'Content-Type' => 'application/json'
  }
)
```

:::

::: response Example response

```json
{
  "id": 2,
  "name": "Dolemon Sushi",
  "description": "Unmissable Japanese Sushi restaurant. The cheese and salmon makis are delicious",
  "created_by": null,
  "updated_by": null,
  "created_at": "2020-08-04T09:57:11.669Z",
  "updated_at": "2020-08-04T09:57:11.669Z",
  "categories": [
    {
      "id": 3,
      "name": "Japanese",
      "created_by": 1,
      "updated_by": 1,
      "created_at": "2020-07-31T11:36:23.164Z",
      "updated_at": "2020-07-31T11:36:23.172Z"
    }
  ]
}
```
:::
::::

### Example

```ruby
require 'httparty'

class Restaurant
  include HTTParty

  def initialize
    @api_url = "http://localhost:1337"
  end

  def all
    self.class.get("#{@api_url}/restaurants")
  end

  def create(params)
    self.class.post("#{@api_url}/restaurants",
      body: {
        name: params[:name],
        description: params[:description],
        categories: params[:categories]
      },
      header: {
        'Content-Type' => 'application/json'
      }
    )
  end

end

restaurant = Restaurant.new
puts restaurant.create({
  name: "Dolemon Sushi",
  description: "Unmissable Japanese Sushi restaurant. The cheese and salmon makis are delicious",
  categories: [3]
})
```

## PUT Request your collection type

Execute a `PUT` request on the `restaurant` Collection Type in order to update the category of a restaurant.

Be sure that you activated the `put` permission for the `restaurant` Collection Type.

:::: api-call
::: request Example PUT request

```ruby
HTTParty.put("http://localhost:1337/api/restaurants/2",
  body: {
    categories: [2]
  },
  header: {
    'Content-Type' => 'application/json'
  }
)
```

:::

::: response Example response

```json
{
  "id": 2,
  "name": "Dolemon Sushi",
  "description": "Unmissable Japanese Sushi restaurant. The cheese and salmon makis are delicious",
  "created_by": null,
  "updated_by": null,
  "created_at": "2020-08-04T10:21:30.219Z",
  "updated_at": "2020-08-04T10:21:30.219Z",
  "categories": [
    {
      "id": 2,
      "name": "Brunch",
      "created_by": 1,
      "updated_by": 1,
      "created_at": "2020-08-04T10:24:26.901Z",
      "updated_at": "2020-08-04T10:24:26.911Z"
    }
  ]
}
```
:::
::::

### Example

```ruby
require 'httparty'

class Restaurant
  include HTTParty

  def initialize
    @api_url = "http://localhost:1337"
  end

  def all
    self.class.get("#{@api_url}/restaurants")
  end

  def create(params)
    self.class.post("#{@api_url}/restaurants",
      body: {
        name: params[:name],
        description: params[:description],
        categories: params[:categories]
      },
      header: {
        'Content-Type' => 'application/json'
      }
    )
  end

  def update(id, params)
    self.class.put("#{@api_url}/restaurants/#{id}",
      body: {
        categories: params[:categories]
      },
      header: {
        'Content-Type' => 'application/json'
      }
    )
  end

end

restaurant = Restaurant.new
puts restaurant.update(2, {categories: [2]})
```

## Conclusion

Here is how to request your Collection Types in Strapi using Ruby. When you create a Collection Type or a Single Type you will have a certain number of REST API endpoints available to interact with.
