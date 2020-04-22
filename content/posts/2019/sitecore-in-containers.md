---
title: Sitecore in Containers
date: 2019-11-13
template: "post"
draft: true
category: "Sitecore"
---

Let me begin by adding a disclaimer to this post:
I am well aware of the Sitecore Docker images repository and the work that the community has been doing in building out that repository for Docker images for Sitecore. This post isn't intended to overshadow or replace the amazing work the community has done. Rather, I figured the best way to learn all the nuances of containers and Sitecore (of which there are many) is to start from scratch and see how far I'd get. This post is documenting that process.

## Anatomy of a Sitecore XP Installation

For anyone who has been working with Sitecore in the recent past, you know that there are a lot of moving pieces are services that come along with Sitecore. My container experiments targeted Sitecore 9.2, and as of 9.2 you have the following processes running:

- Sitecore content management
- Sitecore content delivery
- xConnect web services
- xConnect marketing automation engine
- xConnect processing engine
- xConnect search indexer
- Sitecore identity
- Microsoft SQL Server
- Apache Solr

Wow, that's a lot. Okay, let's break this up by dependencies:

### ASP.NET

- Sitecore content management
- Sitecore content delivery
- xConnect web services

### .NET Framework

- xConnect marketing automation engine
- xConnect processing engine
- xConnect search indexer

### .NET Core

- Sitecore identity

### Java Runtime

- Apache Solr

### Other

- Microsoft SQL Server

So now with this, we get a better understanding of how we can arrange our containers. For the ASP.NET applications, we can use the Microsoft ASP.NET base container, for the .NET Framwork applications the Microsoft .NET Framwork container, etc.

Container images are built on a concept of layers. At its very foundation is the base OS image - this is the very first layer for containers. Additional layers can be added on. With Microsoft-provided images for frameworks such as ASP.NET, Microsoft has included additional layers that perform other actions such as installing IIS, installing .NET Framework, set registry values and permissions, and the like. While you could take the same base OS image and create a Dockerfile to add the same layers, by using one provided by the vendor itself, it provides consistency and an extra layer of caching.

