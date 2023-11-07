---
layout: doc
date: 2023-11-07
title: How to encode JSON into an environment variable and deploy it to a Google Cloud Function using flags.yaml
tags:
- CGF
---
<Title />

In order to use JSON inside an environment variable and deploy it with ```flags.yaml``` you need to change the delimiter that separates the single ENV variables becyuse it defaults to comma and that obviously will create some headaches.

You do so by enclosing you custom delimiter between carets (^) right in front of the list of ENV variables:

```yaml
- --update-env-vars: ^|^ENV1=value1|ENV1=value2|ENV3=value3|JSON_ENV={"key1":"value1","key2":"value2"}
```

<Comment />
