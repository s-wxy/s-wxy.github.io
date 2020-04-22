---
layout: post
title: How to set charset of all text responses on nginx
---
It's usual that all text files on a site share the same character encoding.
Especially [UTF-8][utf8]

```nginx
map $sent_http_content_type $charset {
    ~^text/   utf-8;
}

charset       $charset;
charset_types *;
```

This setting would make nginx specify UTF-8 for all text responses, e.g.
`text/css; charset=utf-8`.


[utf8]: https://en.wikipedia.org/wiki/UTF-8

*[ASCII]: American Standard Code for Information Interchange
*[BOM]: byte order mark
*[regex]: regular expression
