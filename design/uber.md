

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
4. APIs -
   1. GET /ride/cost -> return cost
   2. POST /ride -> {source, destination} -> to create a booking 
   3. POST /location -> {latitude, longitude} -> for driver to send location
   4. PATCH /ride/confirm -> {driverId, rideId, status} -> accept/decline
   5. PATCH /ride -> {driverId, rideId, status} -> ride completed, at pickup location, in progress
5. High Level Design - simple, satisfy all FR
6. Optimise and Deepdive - satisfy all NRF.
