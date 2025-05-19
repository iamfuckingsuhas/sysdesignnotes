# sysdesignnotes

---

## Steps - 

1. Functional Requirement
2. Non Functional Requirements - focus on CAP Theorem, Read Write load, latency, load distribution, fault tolerant, GDPR, security, CI/CD, backups.
3. Estimates 
4. Core Entities - based on func requirements
5. APIs - based on function requirements
6. basic HLD & Flow
7. Deep dive - pick 2/3 from NFR and components and explain how we scale in all of them.
8. Optimisations
9. Verify if all FR & NFR is satisfied
10. Monitoring, Logging and Fault Tolerance


UserId will be fetched from auth / session tokens.

API Gateway does - 
- Authentication
- Routing
- Rate Limiting


Application Later (7) - top level of networking where we deal with application to application interactions like http, websocket, webrtc.
Transport Layer(4)- layer which provides the end to end connection - tcp/udp
Networking Layer (3) - layer handling ip , breaking data into packets, routing and addressing.

TCP - slow but reliable, has 3 way handshake for initation. stateful and connections is streams. packet sent again, 
UDP - fast but unreliable, no guarentee of delivery, no ordering, no handshake required. low latency.

3 way handshake - 
* Client sends a message - SYN (**SYN**chronize) 
* server sends SYN - ACK (Synchronize-Acknowledgement) 
* Client receives SYN - ACK and sends ACK. 
* server recieves ACK and TCP connection is established.
 
TCP teardown - 
* client sends a FIN segment to server (with sequence number)
* server sends a ACK to client (with sequence number + 1) and also a FIN 
* client receives both and sends an ACK


http - request response protocol. stateless.
https - we add a secuity layer for http, we encrypt our communication, protects from eavsdropping and middle of the man attack, requests are safe in transit.

---
rest is a paradigm that is used while building APIs.
in rest clients are performing some operation against a resource. 
- Client/Server
- Stateless
- requests should have their own Cacheablity specified
- Uniform interface - uses xml/json like serialization for input& output.
- Layered system – A client cannot ordinarily tell whether it is connected directly to the end server, or to an intermediary along the way

---
LongPolling - 
- Client makes a request and server keeps it open till it reaches timeout or has data to send, once sent connection is closed.
- A new request is made by the client to reestablish connection once data is received.

SSE 
- server sent events
- servers push data to client in streams/chunks once an initial connection has been established.
- text/event-stream is used.
- one way comm - server -> client

WebSocket - 
- bidirectional connection between client and server,
- client initiates a websocket handshake, the existing connection is upgraded to websocket and client and server can start sending.
- too expensive to maintain

WebRTC - clients can communicate b/w themeselves without intermediaray - using signalling servers.

---
Load Balancer - distribute requests when horizontally scaling

L4 load balancer - transport layer - makes routing decision based on ip address 
1. keep connection between client and balancer and balancer to server alive - good for websocket
2. fast as no inspection required

L7 load balancer - application layer - makes routing decision based on request
1. terminates connection and creates new connection to backend.
2. more flexible, less performance due to packet inspection.

**LB Algos** - 
1. Round robin, weighted RB (server with more weight get more requests), sticky round robin.
2. Random 
3. Least Connections - requests given to servers with least active connections
4. Least response time - requests given to servers with fastest response time (i.e less load ?).
5. IP Hash - requests distributed based on client IP 
---

circuit breaker - if a dependecy fails, it piles up requests making it harder to recover. instead when dependency fails, we stop sending requests. circuit breaker periodicallu checks the dependency and slowly starts sending requests once recovered. like CB trips and interrupts flow and once issue is fixed slowly send all requests.


---
CAP theorem - Consistency, Availability, Partition Tolerance
Consistency - any changes made by one user is immediately shown to other users
Availability - all requests are processed successfully.
Parition Tolerance - partition means network failure between modules. so PT means system works even in case of network failure between modules.

Since networks are unreliable we cannot sacrifice partition tolerance. - we want system to work even in case of network failure. so we have to sacrifice Consistency or Availability.
Simple example - server -> db, if connection fails, we can either serve from cache (availability) or just fail requests (Consistency). 

---
**Consistent Hashsing**  - 

if we have sharded data, we need to recover/migrate in case of failure and we also need to distribute load evenly. we use consistent hashing for this.
hash ring -> place nodes -> go clockwise from request -> place virtual nodes to distribute more evenly.

---

gossip protocol - each node has information about other nodes, get update from one node and passes it to other nodes. to prevent some nodes from not getting, we gossip through a few nodes.

redis AOF - append only file - logs every write operation by server so it can be built in case of failure.

snapshot.

redis sorted sets can be used as a priority queue
redis supports geo hash, each geohash requires approx 8 bytes.

---

Elastic Search 
- in ES index = table, mapping = index.
- good for read heavy search workloads, basically is orchestration for lucene
- uses lucene underneath, lucene uses immutable segments for data.
- shards can have replica.
- supports pagination.
- master nodes, coordinating nodes, data nodes, ingestion nodes
optimisations - node level cache, query planning

---

Kafka
- partition is basically a log file from which messages can be read from. topic is collection of partitions.
- consumer group - each event can be processed by one consumer in the consumer group only.
- kakfa uses messagekey hashing to determine the partition or sends messages in round robin.
- broker - kafka server.
- each message has offset and customers keep tracking their offset and periodically commit it to kafka. incase of failrue they can start from previous offset.
- has master and slave replica. slave will only keep updating and take over incase of failure. master serves read and write.
- partitions managed by consistent hashing, so if a broker goes down, its paritions are redistributed and consumers can read from previous offsets.
- kafka can batch & have retention policy
- good for real time data processing if data is low.
- hot key - randomly distribute messages, parititon hot key further
  
---

Cassandra 
- fast writes(all operations are create only, delete is also soft delete), uses consistent hashing for data & finding replicas.
- tunable consistency - can set how many ACKs required for read & write, if set to Quorum (n/2+1), guarentee each write is seen by each read.
- columnar, no fixed schema, each row is just a mapping of column to value
- uses LSM -> all write append to commit log for durability, all writes made to in memory memTable partitioned by pk. memtabes are periodically flushed to disk as immutable SSTable (sorted string table), uses bloom filter to determine which sstable to be loaded into memory for writing.
- all nodes can coordinate requests, uses gossip protocol for sharing node info through predefined nodes.
- in case of failure, hinted handoffs are used and some other node takes the write and when original node is online, its given.
---

ratelimiter algos - 
1. token bucket
2. queue
3. fixed window counter
4. sliding window counter
5. sliding window approx
rate limiting info sent on response headers.
---
snowflake id - some bits for timestamp - some bits for machine id - some bits for sequence - high throughput and chronologically sortable.

url shortener - base62 encoding of snowflakes 

web crawler - frontier queue -> download step -> parse step -> add to frontier queue. use ratelimiter and db for robots.txt

feed system - generate feed for other users for small number of friends, if celeb, generate on the go.

chat system/realtime notifications - websocket vs sse vs long/short polling - use websocket with chat servers - use redis pubsub for message distribution, L4 LB.

search recommendations - trie - keep topK in trie leaf node - async trie updates - data sampelling

youtube - async transcoding - DAG type - scheduler - task - workers - update status - queue

google drive - blocks - checksum - full file hashing - sync by creating new file, SHA 256 fr chunking and AES - 256 for encryption

proximity service - 
- static - geohash vs quadtree
- geohash - divide square into two by adding one bit - 6 digits 1km and 1mtr upto 8 
- quadtree - recursive till condition is met - tree like - in memory

ticket master / uber - redlock for ticket / driver 

---


zookeeper for managing consistent hashing in custom servers.


