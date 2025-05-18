
1. Functional Requirements
    - get top K most viewed videos in a time period - asked
    - sliding window or tumbling window
2. Non Functional Requirements
    - CAP - Should it be accurate or should it be highly available - < 1 minute.
    -  reads >>> writes.
    -  latency - low latency < 200ms.
    -  should handle millions of view and videos.
3. Core Entities
    - Views
    - Video
    - Window
4. APIs / Interface
    - GET /video?topK={:k}&window={:windowsize} -> list of <videoId, views>
    

   break problem into counting and topK to simplify flow

estimate - 1m videos per day = 365m per year = 3.5B for 10 yrs
= 3.5GB at 1byte per entry, counts will be approx 20 bytes = 80GB so can fit in memory

top k service is stateful, we need to reduce statefulness to scale.

enabling checkpointing where we send backup of current data to s3 at point in time, this reduces catchup work incase of failure.

sharding based on video-id % number of shards to distribute loads 
we need to fetch from all shards and merge them so need a top k service. 

zookeper for service/shard management 

sharded on video-id so all views go to one of the top k worker only. view stream can also be paritioned on video-id.

if a new shard is added, videos need to me moved. to reduce this, we need to use consistent hashing. zookeeper for easy management of shards.


for each time period like 1 hour, 1 day, we keep a sliding window by one consumer reading the latest event and another reading the current-1 hour back lagging events. 
this way both will update counter and keep counts accurate.

![img](https://github.com/iamfuckingsuhas/sysdesignnotes/blob/main/Assets/topkdesign.png)


top k service will fetch from these servers and merge them (merge k sorted lists) fast and get top videos.

   
9. High Level Design & Flow
10. Deep Dive 
