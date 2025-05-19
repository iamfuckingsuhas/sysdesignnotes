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
Load Balancer - 

