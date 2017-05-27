---
layout: post
title: DRY your database.yml
author: Eric Anderson
description: Use YAML's anchor and reference syntax to DRY up your database.yml file.
tags:
- DRY
- config
---
Use anchors (&) and references (\*) to merge options allowing your database.yml to not be so repetitive. Obviously you can organize in a way that makes the most sense to your organization but the following is an example where the database and test environments share some config and the production and staging environment share some config. So rather than repeating them you we just tag the first one with an anchor and then merge it (<<) into the next def with a reference (\*).

    development: &sqlite
      adapter: sqlite3
      database: db/development.sqlite3
      pool: 5
      timeout: 5000
    test:
      <<: *sqlite
      database: db/test.sqlite3
    staging: &mysql
      adapter: mysql
      encoding: utf8
      socket: /tmp/mysql.sock
      username: admin
      password: password
      database: staging
    production:
      <<: *mysql
      database: production
