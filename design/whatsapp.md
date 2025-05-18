
1. Functional Requirements
    - users should be able to send messages - text & multi media
    - users should be able to send messages to 1/group
    - users should be able to recieve or send messages to offline users
    - support multiple DBs
2. Non Functional Requirements
    - Low latency < 500ms
    - Reliable - messages sent should be delivered
    - scale - billions of users & messages
    - fault tolerant - should keep working even if some components fail
    - CAP Theorem , E2E
3. Core Entities
  User
  Message
  Group
  Client

bidirectional so we can use websocket / persistant connection between chat server and user. 
callout at start, this is basic, we will revisit after hld is done as based on approach it can change.
4. APIs
since these are websockets, its more like commands
client - 
createChat
sendMessage
updateChat
uploadFile

server - 
newMessage
chatUpdate

chat server stores mapping - 
Stores Mapping
User1: WS1
User2: WS2

get presigned url, upload to s3 and send url in message. -> explain flow.
have a cron job to delete this data later or keep multi tier storage like cold & warm storage.

for offline users, put message in messageId, instead of creating a new record for each user, we can create a record in message table and have a inbox table.

user will send a ACK after receiving message, we will remove that entry from table.
online offline indicator based on ping pong frame of websocket to see which users are online/offline.

basic hld - 
![img](https://github.com/iamfuckingsuhas/sysdesignnotes/blob/main/Assets/inside/whatsppfinal.png)

7. HLD, DataFlow
use L4 load balancer as connection is persisted on both side
Least Connections

users might be connected to different chat servers, to route, we use pubsub.

for pubsub we can shard based on userId and also have replicas

to save space, we can run a cron job to delete messages periodically.

estimate - 
1B messages per day  = 1B * 100KB = 1000 m * 0.1KB = 100m mb = 100000 GB = 100TB per day 


for multi client users, we have a client table. mapping in chat servers will be based on client id.


final img
![img](https://github.com/iamfuckingsuhas/sysdesignnotes/blob/main/Assets/inside/whatsppfinal.png)


9. Deep Dive 
