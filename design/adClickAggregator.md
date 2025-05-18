Ad click aggregator

- Lambda architecture
- we have two phases -
- one for fast view - stream messages - aggregate - update DB
- one for accurate realtime / verifying above - log messages to append only - run spark job to validate the count periodically.
- realtime/as close as to realtime means stream processing



1. Functional Requirements
    - increment count on view
    - user should be able to view/query in realtime
2. Non Functional Requirements
    - latency - query in less than 5 seconds
    - reliability - views should not be lost
    - Fault tolerance - cached views should be shown in case of failure
    - idempotency of ad clicks - ad clicks should be unique
3. HLD / Flow -
    - click data comes in
    - user redirected
    - click data is validated
    - click data is logged
    - click data is aggregated
    - aggregated data can be queried

clicks could be missed if users use adblocker where click event will be missed, so after click ingestion is done, send redirect url. 

302 for redirect

DB needs a lot of writes so we can use cassandra due to its LSM 
using just the cassandra doesnt work as querying clicks would take a lot of time
we need to aggregate the data like condense it so reads are fast

we can use OLAP DB - read optimised DB like 

final hld - 
![img](https://github.com/iamfuckingsuhas/sysdesignnotes/blob/main/Assets/adcounterprima.png)

kinesis has limit of 1000 per second
shard kinesis by ad id
flink is horizontally scalable, can take more jobs/executors



spark wont work as we need as real time as possible, we use streams with flink/spark stream

aggregation window - period for which flink collects events, performs aggregation and then send final count. 

flush intervals - after this interval, flink will send partial results. - continuous triggers

![img](https://github.com/iamfuckingsuhas/sysdesignnotes/blob/main/Assets/adclickaggregatorsecond.png)

hot shard - some ads can get more clicks

partition - celebrity problem - adId : 0-N -> further dividing popular ad id into zero to n shards

flink can be one job per ad id and since its memory based, it can scale.

enable a few day retention policy 

kafka streams - distributed, fault tolerant and highly available

we use a append only DB and a spark job for reconciliation that runs periodically to keep data accurate.

final - 
![img](https://github.com/iamfuckingsuhas/sysdesignnotes/blob/main/Assets/adaggregatorfinal.png)

