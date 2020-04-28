---
layout: post
title: Protecting your frontend from a bad backend
date: 2020-04-23 09:00:00
author: Matías Lorenzo
tags: [Xmartlabs, Architectural Patterns, Frontend]
author_id: mlorenzo
featured_image: /images/url-splitting/banner.jpg
show: true
category: development
---

As a developer, you'll probably have to consume services provided by code of other developers.
This is true in many aspects of the trade: using a third-party library, interfacing with OS components, consuming web services.
I'd even wager that, in today's development environment, it's highly unlikely that you won't do one of the previously listed things.

Relying on other people's code can be great: it allows us to manage and maintain less code, and also rely on the community for support.
It can also be challenging.
Not all code is great, and not all code works like we want it to.

This post tackles one of such challenges: a frontend app that must consume services and data from a "bad" backend.
We'll look at a way to define the architecture of frontend apps to be resilient against backends that change frecuently, or that don't serve structured data.
Also, by implementing this architecture, your project will be less tightly coupled to the backend, allowing for some freedom of technical choices (keep reading to find out more!).

## Motivation

As a full-stack developer I've had stand on both sides of the table, and I've noticed a big difference on how backends and frontends are approached.
Backend developers are typically encouraged to "protect" their apps by implementing some good practices and patterns.
"Always serialize outbound data", "The MVC pattern provides structure", "Endpoints should do one thing", and more such phrases.
Naturally, how you define a backend's architecture is subjective and depends on the developer/project, but I feel like backends are always treated with some kind of "reverence".
The code must be fast, reliable and secure.
Yet I've failed to find focus on some of these patterns and good practices on the frontend.

Frontends, by nature, have an enormous flaw: they strictly depend on a backend.
This is not the case *always*, I know, but I feel like it's the case most of the time with medium to large apps.
That dependency is a blessing, but also a curse.
If the backend changes its API or re-structures data in some other way, the frontend **has** to be refactored, there's no way around it.

With such a glaring problem, we must.
