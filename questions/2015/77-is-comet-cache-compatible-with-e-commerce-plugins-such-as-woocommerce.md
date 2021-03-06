---
title: Is Comet Cache compatible with E-Commerce plugins (such as WooCommerce)?
categories: questions
tags: themeplugin-developers
author: raamdev
github-issue: https://github.com/websharks/comet-cache-kb/issues/77
toc-enable: off
---

Yes, Comet Cache is compatible with WooCommerce and any other WordPress E-Commerce plugin that utilizes the standard method of notifying WordPress caching plugins that a specific page must not be cached. This is done using the following line of PHP code:

```php
define('DONOTCACHEPAGE', TRUE);
```

WordPress plugins such as WooCommerce, which may generate pages that are _always_ dynamic (such as a 'checkout' page in an e-commerce system), are required to use an agreed upon method of telling any WordPress caching plugins on the site to disable caching for that specific page. This 'agreed upon method' is the `DONOTCACHEPAGE` PHP constant. 

All of the major WordPress caching plugins obey the `DONOTCACHEPAGE` constant--if a WordPress plugin sets `DONOTCACHEPAGE` to true for a particular page, Comet Cache (and any other WordPress caching plugin) will not cache that page.

It's up to each WordPress plugin developer to tell Comet Cache (and any other WordPress caching plugins) which pages are dynamic and must not be cached. WooCommerce already does this (i.e., it already sets the `DONOTCACHEPAGE` constant) on all of the pages that it declares must always remain dynamic and therefore must never be cached.

There's nothing special that you need to configure with regards to Comet Cache and WooCommerce. 

## What if my e-commerce page is being cached?

If there's an e-commerce plugin (or any other WordPress plugin) that has not been properly written to work alongside WordPress caching plugins, or if there's a particular page that you do not want Comet Cache to cache, you can add the page URI to the URI Exclusion Patterns (see **WordPress Dashboard → Comet Cache → Plugin Options → URI Exclusion Patterns**).

When you add the page URI to the Comet Cache URI Exclusion Patterns, Comet Cache will not cache any page that matches that URI.
