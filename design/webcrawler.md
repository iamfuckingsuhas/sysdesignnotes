1. Functional Requirement
    - crawler should download html from seed urls, parse and visit the urls and repeat steps.
2. Non Functional Requirement
    - Reliable - if there is any error, data should not be lost
    - politeness - not too many requests - robots.txt
    - Scale - handle millions of pages
3. Core Entities
WebPage - 
4. APIs / Interface
![initial](https://github.com/iamfuckingsuhas/sysdesignnotes/blob/main/Assets/inside/webcrawlerinit.png)
Flow - 
  - fetch url from seed
  - download and parse webpage
  - add content in storage
  - fetch urls from parsed page
  - add urls to seed 
7. HLD & Flow

crawler is doing a lot so break it into download and parsing step.

DLQ - SQS retry with exponential backoff - exponential visibility backoff. retries can be configured.

set visibility timeout so that other crawlers dont pull the same message. visibility timeout > processing time.

lot of url for deduplication, we can use redis bloom filter check - probabilistic - either we might have or we definetly dont have.

crawler trap - large number of webpages to keep crawler in the site only, avoid this by setting maxdepth.

for webcrawler with continued updatedes, we add a url scheduler that periodically adds urls to queue and also keep updating db like with last modified date.
final
![final](https://github.com/iamfuckingsuhas/sysdesignnotes/blob/main/Assets/inside/webcrawler_final.png)


8. Deep Dive
