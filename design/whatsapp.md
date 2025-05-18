
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
![img](https://github.com/iamfuckingsuhas/sysdesignnotes/blob/main/Assets/inside/image.png)

7. HLD, DataFlow
8. Deep Dive 
