---
title: 'Writing The Server using Hyper Library'
series: 'hff'
description: "Writing the server and routing system for the HTMX fullstack framework. Using hyper library to write the server. Planing file-based routing."
date: "3:03AM,5-11-2023"
tags: ['programming', 'recreational', 'htmx', 'htmx-framework', 'fullstack', 'devlog', 'filebasedrouting', 'hyper', 'http']
---

Today, I'll write the server and configure the filebased routing to some extent.   
Also I forgot to censor r*st in my last blogpost which was dangerous. Git pushed the fix. Also I guess it is fine since "drama driven development" ain't too bad either.

### The Hyper HTTP Library   
Hyper is a low level HTTP library. I could've used Axum or any other higher level library I'm familiar with but I guess I have to use Hyper for obvious reasons.   
