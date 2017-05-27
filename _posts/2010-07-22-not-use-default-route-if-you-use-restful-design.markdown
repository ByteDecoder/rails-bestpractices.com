---
layout: post
title: Not use default route if you use RESTful design
author: Wen-Tien Chang
description: If you use RESTful design, you should NOT use default route. It will cause a security problem. I explain at http://ihower.tw/blog/archives/3265 too.
tags:
- rails2
- route
---
Bad Smell
---------

    map.resources :posts, :member => { :push => :post }

    map.connect ':controller/:action/:id'
    map.connect ':controller/:action/:id.:format'

Why do not use the default route? In this example, you define the resources posts, that means user can only create a post by HTTP POST, update a post by PUT and destroy a post by DELETE. If this is what you expect, default route will be a security problem, because user can create, update or destroy a post by HTTP GET if you define the default route.

Refactor
--------

    map.resources :posts, :member => { :push => :post }

    #map.connect ':controller/:action/:id'
    #map.connect ':controller/:action/:id.:format'

    map.connect 'special/:action/:id', :controller => 'special'

Because default route is evil, just comment out or remove it (if you use RESTful)
