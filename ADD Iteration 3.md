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

| ID   | Type         | Description                                                | Related QA   | Impact                                                           |
|------|--------------|------------------------------------------------------------|--------------|------------------------------------------------------------------|
| R1   | Risk         | External dependency failure (SSO or AI provider)           | Reliability  | Breaks uptime requirement means users are unable to access AIDAP | 
| R2   | Risk         | Microservice crash not detected or recovered quickly       | Reliability  | Increased downtime lead to degraded service continuity           |
| R3   | Risk         | AI inference latency exceeds 2 seconds under load          | Efficiency   | Reduces usability                                                |
| R4   | Risk         | Dashboard and widgets take longer than 2 seconds to load   | Efficiency   | Poor user experience means system feels slow                     |
| R5   | Risk         | Token validation inconsistencies between services          | Security     | Authorization errors can lead to potential unauthorized access   |
| R6   | Risk         | Data not encrypted properly in transit or at rest          | Security     | Risk of data exposure or leak                                    |
| NR1  | Non-Risk     | Microservice isolation prevents entire system failure      | Reliability  | Supports availability and fault containment                      |
| NR2  | Non-Risk     | Cloud auto-scaling handles peak loads effectively          | Efficiency   | Helps maintain performance during spikes                         |
| NR3  | Non-Risk     | SSO authentication ensures consistent identity validation  | Security     | Maintains stable and secure access control                       |
| S1   | Sensitivity  | AI model response time                                     | Efficiency   | Small increases directly break the 2-second requirement          |
| S2   | Sensitivity  | API Gateway overhead                                       | Efficiency   | Added latency affects every request end-to-end                   |
| S3   | Sensitivity  | Token expiry, signature, and validation rules              | Security     | Misconfigurations create authentication vulnerabilities          |
| S4   | Sensitivity  | Autoscaling thresholds                                     | Efficiency   | Incorrect thresholds cause overload or high cost                 |
| T1   | Tradeoff     | Strict encryption and validation measures                  | Security     | Slower processing; hurts efficiency                              |
| T2   | Tradeoff     | Detailed logging and monitoring                            | Security     | Increased latency and performance overhead                       |
| T3   | Tradeoff     | More microservices improve modularity                      | Reliability  | More network hops → higher latency                               |


<h2>Architecture Analysis</h2>

<h3>Sensitivities</h3>
<ul>
  <li><strong>S1: AI model response time (Efficiency):</strong> Small increases in AI inference time can push total response time above the 2-second requirement.</li>
  <li><strong>S2: API Gateway overhead (Efficiency):</strong> Routing, authentication checks, and logging directly affect end-to-end latency.</li>
  <li><strong>S3: Token expiry, signature, and validation rules (Security):</strong> Stricter validation improves security but increases the chance of failed or denied logins.</li>
  <li><strong>S4: Autoscaling thresholds (Efficiency & Reliability):</strong> Scaling triggers (CPU, latency, request rate) determine whether services scale early enough to prevent overload.</li>
</ul>

<h3>Tradeoffs</h3>
<ul>
  <li><strong>T1: Security (+) vs. Efficiency (–):</strong> Stronger encryption and stricter token checks increase security but add processing overhead.</li>
  <li><strong>T2: Security (+) vs. Performance Overhead (–):</strong> Logging, monitoring, and auditing improve security but increase I/O and latency.</li>
  <li><strong>T3: Reliability & Modularity (+) vs. Latency/Complexity (–):</strong> More granular microservices improve isolation but add network hops and architectural complexity.</li>
</ul>

<h3>Risks</h3>
<ul>
  <li><strong>R1: External dependency failure (Reliability):</strong> If SSO or the AI provider fails, users cannot authenticate or send queries.</li>
  <li><strong>R2: Microservice crash not detected or recovered quickly (Reliability):</strong> Failed services may remain offline without proper monitoring, increasing downtime.</li>
  <li><strong>R3: AI inference latency exceeds 2 seconds under load (Efficiency):</strong> Heavy load can slow AI responses and degrade usability.</li>
  <li><strong>R4: Dashboards and widgets load slowly (Efficiency):</strong> Complex aggregation or slow data sources reduce responsiveness.</li>
  <li><strong>R5: Token validation inconsistencies (Security):</strong> Different services validating tokens differently can cause unauthorized access or unexpected denials.</li>
  <li><strong>R6: Improper encryption (Security):</strong> Misconfigured TLS or key management may expose sensitive data.</li>
</ul>

<h3>Non-Risks</h3>
<ul>
  <li><strong>N1: Microservice isolation prevents full system failure (Reliability):</strong> Failure of a single service does not necessarily impact core functionality.</li>
  <li><strong>N2: Cloud auto-scaling handles peak load effectively (Efficiency & Reliability):</strong> Auto-scaling maintains response times and uptime during spikes.</li>
  <li><strong>N3: SSO provides consistent identity validation (Security):</strong> Institutional SSO ensures reliable and mature authentication.</li>
</ul>



