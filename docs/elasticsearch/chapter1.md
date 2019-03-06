# Getting Started With Elasticsearch

## Basic Concepts

### Cluster

* A cluster is identified by a unique name which by default is `elasticsearch`. This name is important because a node can only be part of a cluster if the node is set up to join the cluster by its name.

### Node

* A node can be configured to join a specific cluster by the cluster name. By default, each node is set up to join a cluster named `elasticsearch`

* if there are no other Elasticsearch nodes currently running on your network, starting a single node will by default form a new single-node cluster named`elasticsearch`

### Index

An index is a collection of documents that have somewhat similar characteristics

### Type

A type used to be a logical category/partition of your index to allow you to store different types of documents in the same index, the whole concept of types will be removed in a later version. See [_Removal of mapping types_](https://www.elastic.co/guide/en/elasticsearch/reference/current/removal-of-types.html)

### Document

A document is a basic unit of information that can be indexed, it's expressed in JSON

### Shards and Replicas

An index can be subdivised to multiple pieces called **shards**

Elasticsearch allows you to make one or more copies of your index’s shards into what are called **replica**

By default, each index in Elasticsearch is allocated 5 primary shards and 1 replica which means that if you have at least two nodes in your cluster, your index will have 5 primary shards and another 5 replica shards \(1 complete replica\) for a total of 10 shards per index.

## Exploring the Cluster

### Cluster health

```
GET /_cat/health?v
```

Whenever we ask for the cluster health, we either get green, yellow, or red.

* **Green** - everything is good \(cluster is fully functional\)
* **Yellow** - all data is available but some replicas are not yet allocated \(cluster is fully functional\)
* **Red** - some data is not available for whatever reason \(cluster is partially functional\)

Elasticsearch uses unicast network discovery by default to find other nodes on the same machine, it is possible that you could accidentally start up more than one node on your computer and have them all join a single cluster. In this scenario, you may see more than 1 node in the above response.

```
GET /_cat/nodes?v
```

### List All Indices

```
GET /_cat/indices?v
```

### Create an Index

If you have only one node, the indices created would have yellow health state which means some replicas are not already allocated.

### Index and Query a Document

#### Create The Document

```Json
PUT /customer/_doc/1?pretty
{
  "name": "John Doe"
}
```

Response:

```Json
{
  "_index" : "customer",
  "_type" : "_doc",
  "_id" : "1",
  "_version" : 1,
  "result" : "created",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 0,
  "_primary_term" : 1
}
```

Elasticsearch will automatically create the customer index if it didn’t already exist beforehand.

#### Query the Document

```
GET /customer/_doc/1?pretty
```

Respose:

```
{
  "_index" : "customer",
  "_type" : "_doc",
  "_id" : "1",
  "_version" : 1,
  "_seq_no" : 25,
  "_primary_term" : 1,
  "found" : true,
  "_source" : { "name": "John Doe" }
}
```

### Delete an Index

```
DELETE /customer?pretty
```

## Modifiying Your Data

Reindexig documents using the same id, updates the previously indexed document.

### Updating Documents

Elasticsearch does not actually do in-place updates under the hood. Whenever we do an update, Elasticsearch deletes the old document and then indexes a new document with the update applied to it in one shot.

```
POST /customer/_doc/1/_update?pretty
{
  "doc": { "name": "Jane Doe" }
}
```

Updates can also be performed by using simple scripts. This example uses a script to increment the age by 5:

```
POST /customer/_doc/1/_update?pretty
{
  "script" : "ctx._source.age += 5"
}
```

### Deleting Documents

```
DELETE /customer/_doc/2?pretty
```

See the[`_delete_by_query`API](https://www.elastic.co/guide/en/elasticsearch/reference/6.6/docs-delete-by-query.html) to delete all documents matching a specific query

## Batch Processing

Elasticsearch also provides the ability to perform any of the above operations in batches using the[`_bulk`API](https://www.elastic.co/guide/en/elasticsearch/reference/6.6/docs-bulk.html)

As a quick example, the following call indexes two documents \(ID 1 - John Doe and ID 2 - Jane Doe\) in one bulk operation:

```
POST /customer/_doc/_bulk?pretty
{"index":{"_id":"1"}}
{"name": "John Doe" }
{"index":{"_id":"2"}}
{"name": "Jane Doe" }
```

This example updates the first document \(ID of 1\) and then deletes the second document \(ID of 2\) in one bulk operation:

```
POST /customer/_doc/_bulk?pretty
{"update":{"_id":"1"}}
{"doc": { "name": "John Doe becomes Jane Doe" } }
{"delete":{"_id":"2"}}
```

The Bulk API does not fail due to failures in one of the actions. If a single action fails for whatever reason, it will continue to process the remainder of the actions after it. When the bulk API returns, it will provide a status for each action \(in the same order it was sent in\) so that you can check if a specific action failed or not

