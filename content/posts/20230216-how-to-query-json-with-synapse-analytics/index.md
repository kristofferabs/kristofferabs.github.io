---
title: How to query JSON using serverless SQL Pool in Azure Synapse Analytics
description: Getting familiar with JSON and JSON Lines format and how to query them.
summary: Getting familiar with JSON and JSON Lines format and how to query them.
categories: ["business","technology","data"]
tags: ["sql","json","synapse-analytics"]
date: 2023-02-16
showSummary: true
draft: true
series: ["under the hood"]
series_order: 1
showauthor: false
authors:
  - kristofferabs
---

# How to query JSON using serverless SQL Pool in Azure Synapse Analytics

Getting familiar with JSON and JSON Lines format and how to query them.

> You copy some API data into your lake and store it as JSON using the copy data activity. In your efforts to transform the JSON monster into a beautiful structured table you find yourself banging your head against the error-wall. Not working as you expect you download the file to investigate. You open the json output in VS Code and discover that the JSON format is indeed invalid! 

In this case you are a victim of the default sink format setting property of the copy data activity. The data inside each JSON file will be stored as __setOfObjects__ as default. This will result in JSON Lines (line-delimited JSON files) and will most likely be less familiar to than standard JSON files, __setOfArrays__.

In this post you will get familiar with the two kinds of JSON formats and how we query them using serverless SQL Pool.

## setOfObjects

```json

{
    "name": "John",
    "age": 30,
    "city": "New York",
    "hobbies": [
        "reading",
        "running"
    ],
    "contact": {
        "email": "john@example.com",
        "phone": "555-1234"
    }
}
{
    "name": "Sarah",
    "age": 25,
    "city": "San Francisco",
    "hobbies": [
        "painting",
        "hiking",
        "cooking"
    ],
    "contact": {
        "email": "sarah@example.com",
        "phone": "555-5678",
        "address": {
            "street": "123 Main St",
            "city": "San Francisco",
            "state": "CA",
            "zip": "12345"
        }
    }
}
{
    "name": "Michael",
    "age": 35,
    "city": "Chicago",
    "hobbies": [
        "biking",
        "skiing"
    ],
    "contact": {
        "email": "michael@example.com",
        "phone": "555-9876"
    }
}
{
    "name": "Alice",
    "age": 28,
    "city": "Boston",
    "hobbies": [
        "yoga",
        "reading",
        "traveling"
    ],
    "contact": {
        "email": "alice@example.com",
        "phone": "555-5432",
        "address": {
            "street": "456 Oak St",
            "city": "Boston",
            "state": "MA",
            "zip": "54321"
        }
    }
}
{
    "name": "David",
    "age": 42,
    "city": "Seattle",
    "hobbies": [
        "golf",
        "fishing"
    ],
    "contact": {
        "email": "david@example.com",
        "phone": "555-2222",
        "address": {
            "street": "789 Pine St",
            "city": "Seattle",
            "state": "WA",
            "zip": "67890"
        }
    }
}
```


## setOfArrays

```json

[
  {
    "name": "John",
    "age": 30,
    "city": "New York",
    "hobbies": ["reading", "running"],
    "contact": {
      "email": "john@example.com",
      "phone": "555-1234"
    }
  },
  {
    "name": "Sarah",
    "age": 25,
    "city": "San Francisco",
    "hobbies": ["painting", "hiking", "cooking"],
    "contact": {
      "email": "sarah@example.com",
      "phone": "555-5678",
      "address": {
        "street": "123 Main St",
        "city": "San Francisco",
        "state": "CA",
        "zip": "12345"
      }
    }
  },
  {
    "name": "Michael",
    "age": 35,
    "city": "Chicago",
    "hobbies": ["biking", "skiing"],
    "contact": {
      "email": "michael@example.com",
      "phone": "555-9876"
    }
  },
  {
    "name": "Alice",
    "age": 28,
    "city": "Boston",
    "hobbies": ["yoga", "reading", "traveling"],
    "contact": {
      "email": "alice@example.com",
      "phone": "555-5432",
      "address": {
        "street": "456 Oak St",
        "city": "Boston",
        "state": "MA",
        "zip": "54321"
      }
    }
  },
  {
    "name": "David",
    "age": 42,
    "city": "Seattle",
    "hobbies": ["golf", "fishing"],
    "contact": {
      "email": "david@example.com",
      "phone": "555-2222",
      "address": {
        "street": "789 Pine St",
        "city": "Seattle",
        "state": "WA",
        "zip": "67890"
      }
    }
  }
]
```