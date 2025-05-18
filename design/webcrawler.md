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
8. Deep Dive
