---
title: Why are some scripts being ignored by HTML Compression?
categories: questions
tags: html-compression
author: raamdev
github-issue: https://github.com/websharks/comet-cache-kb/issues/62
toc-enable: off
---

If your site includes a script where the language attribute is supplied (e.g., `<script type="application/ld+json">`, where `ld+json` is the language specification), then the language specified _must_ contain `javascript` (i.e., `<script type="application/javascript">`), otherwise that script is ignored by the HTML Compressor and will not be compressed.

The reason for this is that the HTML Compressor needs intimate knowledge of the language being compressed so that the script does not break after it is compressed. At this time, the HTML Compressor only supports compressing JavaScript, CSS, and HTML. All other languages will be ignored by the HTML Compressor so that there is no risk of breaking your site.
