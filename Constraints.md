| ID | Constraint | Effect |
|:---|:---|:---|
| CON-1 | Average response time ≤ 2 seconds under normal load (RS10) | The interface must provide real-time responses, influencing the design of caching, indexing, and AI inference layers. |
| CON-2 | System availability ≥ 99.5% uptime per month (RS11) | Requires cloud-based redundancy, automated failover, and continuous deployment pipelines to prevent downtime. |
| CON-3 | Single Sign-On (SSO) authentication (RS7) | The system must integrate with institutional SSO providers and ensure once user is logged in once they do not need to again during their session. |
| CON-4 | Data protection and privacy compliance (RS8, RA5) | Enforces secure handling of student and lecturer data in accordance with university and legal privacy policies. |
| CON-5 | Cross-platform support (RS9) | The architecture must support responsive UIs and API compatibility across web, mobile, and voice clients (dynamic css). |
| CON-6 | Integration with existing institutional systems (R3, RA1) | The system must adhere to standard API formats already existing within the institution. |
| CON-7 | Secure data storage and backup policy (RA6, RM6) | Mandates encrypted databases with routine backups and restricted restore access to authorized maintainers. |
| CON-8 | Scalability limit – 5,000 concurrent users (RA7) | Defines minimum capacity planning and cloud resource allocation for scaling. |
| CON-9 | AI model hosting and update constraints (RM3, RM5) | AI models must be updated through approved CI/CD pipelines to avoid service interruptions. |
