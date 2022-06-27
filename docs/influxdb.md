# Concepts
## Measurement 
The measurement acts as a container for tags, fields, and the time column, and the measurement name is the description of the data that are stored in the associated fields. Measurement names are strings, and, for any SQL users out there, a measurement is conceptually similar to a table.

A single measurement can belong to different retention policies. A retention policy describes how long InfluxDB keeps data (DURATION) and how many copies of those data are stored in the cluster (REPLICATION). If you’re interested in reading more about retention policies, check out Database Management.

### Attention
Replication factors do not serve a purpose with single node instances.

`autogen` retention policy. InfluxDB automatically creates that retention policy; it has an infinite duration and a replication factor set to one.
## Fields
Fields are actual data and are not indexed, so queries that uses fields as filters scan over all the data and thus are not performant

Fields are required

## Tags
Tags are made up of tag keys and tag values. Both tag keys and tag values are stored as strings and record metadata

Tags are not required


Tags are indexed. This means that queries on tags are faster and that tags are ideal for storing commonly-queried metadata.

## Tagsets
Tagset is the different combinations of all the tag key-value pairs.

## Series
a series is the collection of data that share a retention policy, measurement, and tag set ().

## Point
a point is the field set in the same series with the same timestamp

## Database
An InfluxDB database is similar to traditional relational databases and serves as a logical container for users, retention policies, continuous queries, and, of course, your time series data

# Getting started with InfluxDB OSS

## Moving average with time aggregation
file:///home/mohammedi/websites/influxdb/docs.influxdata.com/influxdb/v1.7/query_language/functions/index.html#examples-of-advanced-syntax-16

## Querying data
1. Select statemants should contain at least one field.
2. GROUP BY time() queries don't return timestamps that occur after now().
3. Time pretty: precision rfc3339
4. Moving average with N >= number of messages, returns nothing


## Queries
select moving_average(mean(hr),3) from hr_bucketed where time >= '2019-02-10T16:53:45Z' and time <= '2019-02-17T16:53:45Z' group by bucket,time(1d)
