---
layout: post
title:  "Add data to Elasticsearch directly"
date:   2022-6-25 15:19:23
categories: elasticsearch curl
---

I have found a usage where Filebeat or other Elasticsearch exporters are not needed but where I wanted to use Elasticsearch for the index functions but with manually adding data. Manually adding data was not easy so here's how I did it with the Elasticsearch API.

## Step 0

Curl needs to be installed.

## Step 1

In the Document API's there does not seem to be a way to add data directly in the API with a PUT but 