---
date: 2017-04-15T14:39:04+02:00
title: Services
url: services

next_steps:
  - file: mysql.md
    name: Example using MySQL
  - file: postgres.md
    name: Example using Postgres
  - file: redis.md
    name: Example using Redis

menu:
  usage:
    weight: 6
    identifier: services
    parent: usage_concepts
---

Drone provides a services section in the Yaml file used for defining service containers. The below configuration composes database and cache containers.

```diff
pipeline:
  build:
    image: golang
    commands:
      - go build
      - go test

services:
  database:
    image: mysql

  cache:
    image: redis
```

Services are accessed using custom hostnames. In the above example the mysql service is assigned the hostname `database` and is available at `database:3306`.

# Configuration

Service containers generally expose environment variables to customize service startup such as default usernames, passwords and ports. Please see the official image documentation to learn more.

```diff
services:
  database:
    image: mysql
+   environment:
+     - MYSQL_DATABASE=test
+     - MYSQL_ALLOW_EMPTY_PASSWORD=yes

  cache:
    image: redis
```

# Initialization

Service containers require time to initialize and begin to accept connections. If you are unable to connect to a service you may need to wait a few seconds or implement a backoff.

```diff
pipeline:
  test:
    image: golang
    commands:
+     - sleep 15
      - go get
      - go test

services:
  database:
    image: mysql
```
