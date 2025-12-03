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
  <li><strong>S1:</strong>
      Small increases in AI inference time can immediately push total response time over the 2-second requirement, directly affecting conversational usability.
  </li>

  <li><strong>S2:</strong>
      The amount of processing (routing, auth. checks, logging) performed by the API Gateway directly affects end-to-end latency for every request.
  </li>

  <li><strong>S3:</strong>
      How strictly tokens are validated (expiry window, signing algorithms, audience checks) determines both security strength and the likelihood of failed logins or denied requests.
  </li>

  <li><strong>S4:</strong>
      The rules used to trigger scaling (CPU, latency, request rate) determine whether services scale early enough to avoid overload or too late, causing timeouts and degraded reliability.
  </li>
</ul>

<h3>Tradeoffs</h3>
<ul>
  <li><strong>T1: Security (+) vs. Efficiency (–)</strong><br>
      Stronger encryption, stricter token validation, and additional security checks improve protection of academic data and conversations but increase processing time and latency at the API Gateway and services.
  </li>

  <li><strong>T2: Security (+) vs. Performance Overhead (–)</strong><br>
      Detailed logging, monitoring, and audit trails improve incident response and forensics but add I/O and storage overhead, slightly increasing request latency and resource consumption.
  </li>

  <li><strong>T3: Reliability & Modularity (+) vs. Latency/Complexity (–)</strong><br>
      Splitting AIDAP into more granular microservices improves fault isolation and independent deployment, but each extra network hop and service boundary adds latency and architectural complexity.
  </li>
</ul>

<h3>Risks</h3>
<ul>
  <li><strong>R1:</strong>
      If the institution’s SSO service or external AI provider is unavailable, users may be unable to authenticate or send queries, breaking the uptime expectations for AIDAP.
  </li>

  <li><strong>R2:</strong>
      If monitoring or restart mechanisms fail, a crashed AI, dashboard, or auth. service can remain down, increasing downtime and violating availability goals.
  </li>

  <li><strong>R3:</strong>
      Under peak usage, AI services may respond too slowly, causing end-to-end response times to exceed the target and degrading conversational experience.
  </li>

  <li><strong>R4:</strong>
      Complex dashboard aggregation or slow data sources can cause the UI to feel sluggish, reducing usability and student satisfaction.
  </li>

  <li><strong>R5:</strong>
      If services validate tokens differently, users may unexpectedly gain or lose access, potentially leading to unauthorized access or confusing failures.
  </li>

  <li><strong>R6:</strong>
      Misconfiguration of TLS, key management, or storage encryption can expose sensitive institutional and personal data to interception or leakage.
  </li>
</ul>

<h3>Non-Risks</h3>
<ul>
  <li><strong>N1:</strong>
      Because services are independently deployed, a failure in one (e.g., analytics) does not necessarily bring down core query or dashboard functionality.
  </li>

  <li><strong>N2:</strong>
      When configured correctly, the underlying cloud platform can automatically add instances during spikes, helping maintain response time and uptime targets.
  </li>

  <li><strong>N3:</strong>
      Relying on the institution’s standard SSO solution ensures mature, well-tested identity management and reduces the chance of bespoke authentication errors.
  </li>
</ul>


