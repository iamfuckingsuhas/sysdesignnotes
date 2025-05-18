Ad click aggregator

- Lambda architecture
- we have two phases -
- one for fast view - stream messages - aggregate - update DB
- one for accurate realtime / verifying above - log messages to append only - run spark job to validate the count periodically.





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
