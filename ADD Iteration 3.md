<h1>Iteration 3 (ATAM)</h1>

<h2>Selecting Quality Atrributes</h2>

| Quality Attribute     | Rationale                                                                                                                                           |
|-----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------|
| Reliability (QA-1)    | AIDAP is a primary access point to institutional information for students, lecturers, and administrators. Failures directly degrade user trust and operation. |
| Efficiency (QA-4)     | The digital assistant should provide real-time conversational responses. Delays greater than 2 seconds degrade usability.                                |
| Security (QA-5)       | The system handles sensitive academic records, personal data, role-restricted information, historical conversations, and authentication tokens.          |

<h2>Project assessment - ATAM</h2>
ATAM evaluates how the AIDAP architecture supports the three key quality attributes as outlined above. AIDAP uses a cloud native microservices architecture with a Webb app client, API gateway and independent services. The structure established in iterations 1 and 2 form the base of the assessment.
<br><br>

**Reliability** is driven for 99.5% uptime and auto recovery. Microservice isolation, monitoring, and distributed deployment help support this goal, but reliance on external systems introduce potential points of failure

**Efficiency** requires responses in less than 2 secs. Latency is affected by AI interference time, API Gateway routing, cross-service communication, and external call. These areas present the risks under high load.

**Security** depends on correct SSO integration, token validation, encrypted communication and role-based access. The architecture has a strong foundation for these already, but token propagation and cross-service trust boundaries must be evaluated to ensure data confidentiality and access-control.

<h2>ATAM utility tree</h2>
To guide the evaluation, we first represent the key quality drivers in an ATAM Utility Tree.<br>
<img width="1345" height="976" alt="image" src="https://github.com/user-attachments/assets/401a36cb-ca2b-4818-b29c-873a23cf909c" />

<h2>ATAM risk assessment table</h2>

<h2>Risks, non-risks, sensitivity, and tradeoffs</h2>


