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
