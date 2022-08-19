---
layout: default
title: Elastic Search
parent: Tips
---

# Elastic Search

In AWS, it is known as `open-search`.


## Migrate Data

**[General Steps]**

1. use ssh-tunneling for both source and detination service.

1. use `elasticdump` to migrate mapping and data.


### Using elasticdump

install tool by `npm install -g elasticdump`.

```sh
# run example. (may not work due to SSL)
elasticdump --input=http://source.io:9200/my_index --output=http://target.io:9200/my_index --type=mapping
elasticdump --input=http://source.io:9200/my_index --output=http://target.io:9200/my_index --type=data
```

* Refer to `xxxxx-backend-api` for more information.
