+++
title = "Caching: How to Improve your Application's Performance (with Redis)"
description = "A quick guide on how to use caching in web apps"
date = 2025-02-05
draft = false

[taxonomies]
categories = ["performance"]
tags = ["caching", "redis", "ruby", "performance"]

[extra]
lang = "en"
toc = true
comment = true
copy = true
math = false
mermaid = false
display_tags = true
truncate_summary = false
featured = false
reaction = false
+++

Introduction
================

Caching is the practice of storing previously calculated results to avoid the need for subsequent calculations, saving time and computational resources 🚀

There are several ways to implement caching, from the use of in-memory variables to more robust and reliable solutions, such as dedicated databases. These services manage and store data efficiently.

One of the most popular caching systems is Redis, and this is the one we will use in this guide.

We will explore in practice how to implement caching and apply this technique in production environments. Although we will use Ruby to make the explanation easier, the concepts can be applied in any language.

Setup
==============

### What you will need 📝

* Programming language (we will use Ruby)
&nbsp;

* Docker (to run the Redis server) 🐋
&nbsp;

Now that Docker is configured and installed on your machine, let's run the following command:

```shell
docker run -d -p 6379:6379 -i -t redis
```

This command starts the Redis server using the latest version of the official image.

After running, we can check if the server is running with:

```shell
docker ps
```

The expected output will be something similar to this:

```shell
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                                       NAMES
986bb85320f8   redis     "docker-entrypoint.s…"   11 seconds ago   Up 10 seconds   0.0.0.0:6379->6379/tcp, :::6379->6379/tcp   focused_jang
```

Note that the `STATUS` column should contain the word `Up`, indicating that the server is running.

Creating the caching file
================

Now, let's create a file in Ruby to implement caching. We chose Ruby for its didactics, but you can use any other language.

### Code structure

```ruby
require "redis"

CACHE_KEY_ID = "my-cache"
REDIS = Redis.new(host: "127.0.0.1", port: 6379)

stored_value = REDIS.get(CACHE_KEY_ID)

if stored_value.nil?
 stored_value = Net::HTTP.get(URI("https://www.example.com"))
REDIS.set(CACHE_KEY_ID, stored_value, ex: 3600)
end

stored_value
```

### Code explanation

#### Initialization and configuration:

```ruby
require "redis"

CACHE_KEY_ID = "my-cache"
REDIS = Redis.new(host: "127.0.0.1", port: 6379)
```

- We import the `redis-rb` library, which allows communication with Redis. In Python, for example, the equivalent library would be `redis-py`
- We define CACHE_KEY_ID, the key used to store and retrieve data in the cache. This key serves as a unique identifier, similar to an ID in a database
- We create the Redis instance, configuring the IP (127.0.0.1) and the port (6379), which are the same values ​​used when running the server in Docker

#### Caching logic:

```ruby
stored_value = REDIS.get(CACHE_KEY_ID)

if stored_value.nil?
stored_value = Net::HTTP.get(URI("https://www.example.com"))
REDIS.set(CACHE_KEY_ID, stored_value, ex: 3600)
end

stored_value
```

Here we perform two main operations: reading and writing to the cache.

1. Cache verification

- First, we try to get the value stored in Redis.
- If the value **already exists**, it will be used and we avoid making a new request.

2. Storing the value

- If the value is not stored (`nil`), we fetch the HTML page by making a `GET` request
- Then, we store the content in Redis with an expiration time of 1 hour (`ex: 3600`)
- This way, new requests within this period will reuse the saved value, avoiding unnecessary requests to the external site

3. Returning the value

- At the end, we return `stored_value`, which will contain the cached value or, if necessary, the newly fetched content.

## Benefits 🚀

In our example, using a cache significantly improves application performance because:

- It avoids repeated requests to external services, reducing response time.
- It ensures that data is accessed more quickly (user experience).

This strategy allows the application to respond much more efficiently to consumers.

Caching Uses
================

✅ When to Use

- Data that changes little (e.g. product lists, settings)
- External APIs with request limits (e.g. exchange rates, weather forecasts)
- Heavy database queries (e.g. reports, dashboards)
- Data frequently accessed by many users (e.g. promotional banners)

❌ When Not to Use

- Data that changes constantly (e.g. order status, real-time chats)
- When accuracy is essential (e.g. bank balance, inventory)
- Sensitive or temporary data (e.g. authentication tokens)

Conclusion
================

If you have any questions, queries or suggestions, don't hesitate to contact me 😊

> REDIS.set("new-learning-in-caching", this.post)

Thanks for reading!! 🍻
