---
title: Handling CORS in Rails for client side apps
date: "2019-01-20T21:20:50.129Z"
layout: post
draft: false
path: '/posts/CORS-in-Ruby-On-Rails'
category: "ruby-on-rails"
tags:
  - "Rails"
  - "CORS"
  - "Javascript"
  - "ruby"
  - "Ruby on Rails"
description: "Handling CORS in Rails for JS apps using React, etc"
---

Building a client side app with React is a bliss, just like building backend with Ruby on Rails. In React if you make an api to call to backed you would get an error like so.

```
Response to preflight request doesn’t pass access control check: No ‘Access-Control-Allow-Origin’ header is present on the requested resource. Origin ‘http://localhost:4000' is therefore not allowed access. The response had HTTP status code 404. If an opaque response serves your needs, set the request’s mode to ‘no-cors’ to fetch the resource with CORS disabled.
```

CORS or Cross Origin Resource Sharing gives web server cross domain access control, which enables secure cross-domain data transfers. Browsers use CORS in an API container, such as [XMLHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) (what [Axios](https://github.com/axios/axios) and [Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) use under the hood), to mitigate the risks of cross-origin HTTP requests.

To fix this, we have 2 options.

1. Set request mode to 'no-cors' to fetch the resource with CORS disabled, this will return an opaque response.
2. Add an ‘Access-Control-Allow-Origin’ header to rails server.

If we set 'no-cors' it returns the following Opaque response.
```js
Response {
  body: null
  bodyUsed: false
  headers: Headers
  ok: false
  redirected: false
  status: 0
  statusText: ""
  type: "opaque"
  url: ""
}
```
There are a couple of things to note:
- the `status` is "0" (and not a regular HTTP status code - like 200)
- the `statusText` is empty
- the `headers` object is also empty
Basically, we can't see anything in this `Response` object - and henceforth the name opaque. Opaque response or 'no-cors' mode can be used with `<script>`, `<img>`, `<video>`, `<audio>`, `<link rel="stylesheet">`, `<object>`, `<embed>` and `<iframe>` elements.

So it's of no use to us, given that we need the data from our rails api. We need to add the `Access-Control-Allow-Origin` header to the rails server. We can use this wonderful gem [rack-cors](https://github.com/cyu/rack-cors). We need to add the gem to our Gemfile.

```ruby
gem 'rack-cors'
```
And add the following config in `config/application.rb` file

```ruby
module YourApp
  class Application < Rails::Application
    # ...

    # Rails 5

    config.middleware.insert_before 0, Rack::Cors do
      allow do
        origins '*'
        resource '*', headers: :any, methods: [:get, :post, :options]
      end
    end

    # Rails 3/4

    config.middleware.insert_before 0, "Rack::Cors" do
      allow do
        origins '*'
        resource '*', headers: :any, methods: [:get, :post, :options]
      end
    end
  end
end
```

That's about it, no more CORS issue. 🚀 We are good to go.