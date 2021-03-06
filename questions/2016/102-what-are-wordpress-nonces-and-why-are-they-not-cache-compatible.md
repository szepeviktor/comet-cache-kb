---
title: What are WordPress nonces and why are they not cache-compatible?
categories: questions
tags: error-messages, themeplugin-developers, bbpress, buddypress, akismet
author: raamdev
github-issue: https://github.com/websharks/comet-cache-kb/issues/102
---

[WordPress nonces](https://codex.wordpress.org/WordPress_Nonces) are WordPress security tokens, a "number used once" that is used to help protect against several types of attacks. Nonces should _never_ be cached, because they are user-specific time-sensitive values.

If an nonce value is cached and it later expires, actions that you attempt to make while working on your site could start to fail. If a nonce value is cached, any actions that depend on that nonce value remaining dynamic are not going to see what they expect to see, which can lead to all sorts of weird problems.

WordPress nonces are used extensively by the WordPress Dashboard to enhance security, but they are also used by WordPress plugins. Plugins like bbPress may inject user-specific action links onto the front-end of the site to allow users to take various actions. Each one of those links could contain a nonce value.

## Allowing Nonce Caching (Not Recommended)

There are two ways you can enable nonce caching: globally (all users and visitors) or only for logged-in users. While we don't recommend _ever_ allowing nonce values to be cached, if you're going to enable nonce caching the safest option is to only enable it for logged-in users.

When Logged-In User caching is enabled (**Dashboard → Comet Cache → Plugin Options → Logged-In Users**), each user has their own unique cache file and that means if a nonce value gets cached, it will only be cached in their own cache file. Other visitors to the site will never see the cached nonce value.

However, if you choose to enable nonce caching globally, then a visitor to your site could be served a cache file generated by another visitor that contains a nonce value intended for someone else. That's opening the door for potential security issues. Not good.

### Logged-In Users (safer)

_Note: Logged-In User caching is disabled by default in Comet Cache. If you leave Logged-In User caching is disabled, the snippet below will have no effect._

If you're using Logged-In User caching and you want to allow nonce values to be cached for Logged-In Users, you can do so by adding the following somewhere near the top of your `wp-config.php` file (after the `<?php` line):

```php
define('COMET_CACHE_CACHE_NONCE_VALUES_WHEN_LOGGED_IN', TRUE);
```

One important thing to keep in mind if you choose to allow nonce caching is the nonce lifetime and how that relates to your Comet Cache Cache Expiration time. See below for further details.

### Globally (dangerous)

If you choose to override the default Comet Cache behavior and allow nonce caching at all times, you can do so by adding the following somewhere near the top of your `wp-config.php` file (after the `<?php` line):

```php
define('COMET_CACHE_CACHE_NONCE_VALUES', TRUE);
```

One important thing to keep in mind if you choose to allow nonce caching is the nonce lifetime and how that relates to your Comet Cache Cache Expiration time. See below for further details.

## WordPress Nonce Lifetime

WordPress nonces have a default lifetime of 1 day from the time they were created. If you choose to allow nonce caching, the best way to avoid issues with caching expired nonce values is to set your cache expiration (**Dashboard → Comet Cache → Plugin Options → Directory / Expiration Time**) to 1 day or less.

The default Cache Expiration time is 7 days, which means that a cache file will not be recreated for at least 7 days. If you are allowing Comet Cache to cache nonce values, a 7 day cache expiration is too long because an expired nonce value could be cached for a full 6 days, which is plenty of time to cause problems for users.

To reduce the probability of caching an expired nonce value, set your cache expiration to 1 day or less.

## Akismet Comment Nonce

The popular [Akismet](https://wordpress.org/plugins/akismet/) plugin deserves special mention here because of how it uses nonces. The Akismet Comment Nonce is a _passive_ comment nonce, which means that the plugin adds a nonce field to the comment form and the result of the nonce check is then passed to the Akismet server to help look for abuse.

The passing or failing of the nonce value is just an additional data point that Akismet takes into consideration when looking for spam. However, caching the Akismet Comment Nonce could add to the overall spam score whenever that cached value is being submitted to the Akismet service, which could result in an increase in false-positives (legitimate comments being marked as spam).

The Akismet plugin provides a filter to other plugin developers so that the Akismet Comment Nonce can be disabled specifically for scenarios like caching. Comet Cache uses this filter to disable the Akismet Comment Nonce, thereby allowing caching to behave as expected.

If for some reason you want to allow Akismet Comment Nonce caching (not recommended), see [this article](http://cometcache.com/kb-article/how-do-i-prevent-comet-cache-from-disabling-the-akismet-comment-nonce/).
