

1. Functional Requirements -
    - User should be able to enter location and desitnation and get fare
    - User shoudl be able to request a ride
    - Drivers should be able to accept/deny the ride
    - Out of scope - 
        - multiple car types 
2. Non Functional Requirements - identify the qualities relevant to system, quantify them
    - should be able to match rides in less than a minute
    - CAP theorem - consistency for matching - ride should be assigned to one driver only, availability for others
    - handle surges for peak hours like during events / weekends - 100k in a region
    - out of scope -
        - GDPR,
        - user privacy
        - Fault Tolerance
        - Monitoring, Logging and Alarms
        - CICD
3. Core Entities -
    1. Driver
    2. User
    3. Booking
    4. Location
4. APIs - userIds will be taken from token instead of passing from header.
   1. GET /ride/cost -> return cost
   2. POST /ride -> {source, destination} -> to create a booking 
   3. POST /location -> {latitude, longitude} -> for driver to send location
   4. PATCH /ride/confirm -> {driverId, rideId, status} -> accept/decline
   5. PATCH /ride -> {driverId, rideId, status} -> ride completed, at pickup location, in progress
5. High Level Design - simple, satisfy all FR
![img](https://github.com/iamfuckingsuhas/sysdesignnotes/blob/main/Assets/basicHldUber.png)
Location DB calc can be optimised -

estimate on number of api calls -

Simplest - 2D search in postgres - not optimised

GeoHash - redis geohash or ourown - we would be using neighbouring 

Quadtree - in memory tree based

cannot use relational DB as requests are too high, so use redis.

GeoSpatial Index - makes querying faster.

PostGIS uses quadtree - not that optimal as tree needs to be updated everytime driver location changes.

geohash - base32 encoding of the location in geohash bits. redis has approx 64 bit/ 8 bytes per geohash

geohash - fast updates.
quadtree - good for uneven distribution, complex updates.


7. Optimise and Deepdive - satisfy all NRF.
