---
title: Schema Design
menu:
  influxdb_010:
    weight: 40
    parent: concepts
---

>TODO: Update with schema suggestions for 0.10.0, tsm. Single measurement, many fields and tags. 

In the 0.10.x version of InfluxDB, it is recommended that you encode metadata into `Tags`.
Tags are indexed within the InfluxDB system allowing fast querying by 1 or more tag values.
Note that tag values are always interpreted as strings.

Starting in the 0.10.x line it is recommended to store more than one field per point and to have as few measurements as possible. 

It’s also a good idea to start the tag names and measurement names with a character in [a-z] or [A-Z], but not a requirement.
It will just make writing queries easier later since you won’t have to wrap the names in double quotes.

Take a common example from the world of computer infrastructure monitoring.
Imagine you need to record CPU load across your entire deployment.
Furthermore, each CPU is actually composed of two cores, numbered 0 and 1.
In this case you could log 4 datapoints into InfluxDB as follows:

```
cpu_load,host=server01,core=0 value=0.45 1437171724
cpu_load,host=server01,core=1 value=1.56 1437171724
cpu_load,host=server02,core=0 value=0.72 1437171724
cpu_load,host=server02,core=1 value=2.14 1437171724
```

With the data in this format, querying and aggregating by various dimension is straightforward -- filter by tags as necessary.
For example, to see only CPU load information from `server01` simply add `host='server01'` to your query.
This would return data for both cores on that machine.
To only see data from core 1, add `host='server01',core='1'`.
And so on.
