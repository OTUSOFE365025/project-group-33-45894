System Constraints

| ID | Constraint | Effect |
|:---|:---|:---|
| CONS-1 | Average response time ≤ 2 seconds under normal load (RS10) | The interface must provide real-time responses, influencing the design of caching, indexing, and AI inference layers to ensure smooth interaction. |
| CONS-2 | System availability ≥ 99.5% uptime per month (RS11) | Requires cloud-based redundancy, automated failover, and continuous deployment pipelines to prevent downtime. |
| CONS-3 | Single Sign-On (SSO) authentication (RS7) | The system must integrate with institutional SSO providers and ensure that once a user logs in, they remain authenticated for the duration of the session. |
| CONS-4 | Data protection and privacy compliance (RS8, RA5) | Enforces secure handling of student and lecturer data in accordance with institutional and legal privacy policies, including encryption and access control. |
| CONS-5 | Secure data storage, backup, and integration policy (RA1, RA6, RM6) | Mandates encrypted databases, routine backups, and adherence to institutional API standards for system interoperability and recovery. |
| CONS-6 | AI model scalability and update constraints (RM3, RM5, RA7) | AI models must support up to 5,000 concurrent users and be updated through approved CI/CD pipelines to avoid service interruptions while maintaining performance and reliability. |
