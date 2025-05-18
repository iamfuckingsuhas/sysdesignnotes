


Ticket master - 
FR - 
1. book a ticket
2. view current event
3. search for events

NFR - 
1. Strong consistency for booking tickets. 
2. High availability for search and viewing events. 
3. reads. >>> writes for searches and viewing. 
4. scalability - surges from frequent events. 
5. low latency search


out of scope - 
1. GDPR compliance
2. Fault Tolerance



Core Entities - 
too early, after moving to HLD, ill clearly define the entities

Events : name, desc, content
Venue 
Ticket  
User 

we get userid/modified by from tokens in headers instead of in body. 

APIs - 
GET /event/:eventId -> capacity, date time, location, duration and other features like parking, food
GET /event/search?term={}&startDate={}&endDate={} -> return list of events

booking a ticket - ticket is temporarily reserved for 10 minutes and then payment is processed and if it fails, the ticket is auto released else confirmed for the user. 

POST /ticket/reserve -> intent, reserving ticket for sometime, initiating paymetn after this
body -> {ticketId}
POST /ticket/confirm -> confirming after payment is done
body -> {ticketId, paymentInfo}

High Level Design - 


Event Table. - 
event id 
event name
performer id
description
venueId
seating 
.....

Venue 
venue id 
venue name 
address 
latitude
longitude
......

Performer
performer id 
performer name
......
specify what properties your DB needs and suggest which db to use for it. 
Ticket - we can use SQL db for this because we need ACID property 
ticketId
ticketStatus
eventId
seatId
price



payment process -> we integrate payment processor's sdk in our UI which renders the payment UI , handle payment and then return a token. 
we send request to backend with this token, backend calls payment processor with token and get the payment status. 

to keep temporary reservation for 10 mins only, 
- we can use reservedAt timestamp and modify to include where status is reserved and was reservedAt more than 10 mins ago 
- we can use a cron job to remove entries periodically
- we can use a redis distributed lock with ticketId and ttl of 10 mins 
- when redis goes down, in the 10 mins period, the users who completed payments first will get the seat but other users might also be shown the tickets. 
- with redlock, reserved status is handled by redis, so ticket will have available or booked only

for deep dive look at NFR and FR and whats missing. 
with caches, explain eviction, expiry and invalidation

since a lot of users, we can throttle then with virtual queue like redis sorted set, open a socket and once tickets are available, poll and update in websocket. 

basic - 


CDC - stream 
optimise search - use elastic search 
elastic search has a limit on index updates, we can use queue to throttle them. 
to make it even faster, we can cache on inputs with redis and CDN 
CDN is good for static assets but for apis like search - its rarely useful for personalized or general purpose searches. 


seat map will get stale if users are on the page for too long for popular evens, so we need to keep the client in loop. 
- long pooling works as users wait for sometime only on the page
- this is a uni directional event so a SSE instead of websocket which is bidirectional

scaling - if a large number of users, we might blackout so use a waiting queue like redis sorted set - like pq to ask users to wait and then let them one by one. 

reads >>> writes - cache in event, venue, performer. 

event, venue and performer can be stored in DDB 
ticket needs ACID - mySQL 

![[https://github.com/iamfuckingsuhas/sysdesignnotes/blob/main/Assets/ticketmaster.png]]


redlock - quorum approach - n/2+1 nodes need to ACK for lock to be acquired.
