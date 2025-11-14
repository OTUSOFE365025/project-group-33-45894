<h1>Iteration 1</h1>
In this iteration, our goal is to create a general overview of the AIDAP system.<br>

<h2>Step 1: Review Inputs</h2>
First, we need to review the inputs. The table below summarizes the system and the purpose of this ADD process

| Category       | Details |
|----------------|---------|
| Design Purpose | The AI Digital Assistant Platform (AIDAP) is a **greenfield system from a mature domain**. As LMS integration, registration systems, calendar systems, and email systems are well understood. <br><br>The **purpose** of this design is to identify the optimal frameworks, tools, components, and methods for designing AIDAP. |



<h2>Step 2: Establish Iteration Goal</h2>
During our first iteration, all drivers will be kept in mind, especially:<br>
<ul>
  <li>QA-1: Reliability</li>
  <li>QA-3: Portability</li>
  <li>QA-4: Efficiency</li>
  <li>CONS-6: AI model scalability and update constraints</li>
  <li>CRN-1: Integration with Institutional Systems</li>
</ul>

<h2>Step 3: Decompose</h2>
Since this is a Greenfield system, we will decompose the entire system.
The AIDAP system will integrate with external University Systems (LMS, Registration, Email). The system will also use external AI services.<br>
<img width="745" height="449" alt="image" src="https://github.com/user-attachments/assets/574a9772-a4a0-426f-b7e7-fb9d1f36393d" />



<h2>Step 4: Choose Design Concept(s) </h2>
The following table summarizes the design concepts selected in this first iteration.

| Design Decisions and Location | Rationale |
|-------------------------------|-----------|
| Structure the client part of the system using the **Web Application** reference architecture | A web app is our best option, as we need AIDAP to be portable and accessible through any device, with limited offline operation (QA-3). Modern web apps also support rich interfaces, which we will need for personalized dashboards (UC-1). <br><br>We **discarded rich client and mobile applications** because they do not offer portability and cannot reliably support multiple AI models or effectively serve thousands of clients (QA-1, CRN-4, CONS-6). We also discarded rich internet applications because they rely on outdated technologies and modern web apps already provide rich interfaces. |
| Structure the server part of the system using a **Cloud Native Microservices** reference architecture | Cloud-native microservices are the most suitable choice (CRN-4, QA-1). Independent services avoid the limitations of a single large service (QA-2). Each service handles a distinct function: AI integration (UC-4), personalized dashboard (UC-1), authentication (UC-5), context memory (CRN-6), and more. This architecture scales efficiently to 5000+ clients and multiple AIs (QA-1, CONS-6). <br><br>We **rejected N-tier, Monolithic, and Serverless architectures**: N-tier cannot scale individual components like AI independently (CRN-4, QA-4), Monolithic requires redeploying the whole system on updates (QA-2, CONS-6), and Serverless cannot maintain AI models in memory or sustain personalized responses (CRN-5, QA-4). |
| Physically structure the diagram using the **three-tier deployment** pattern | We selected a distributed three-tier deployment because AIDAP uses cloud-native microservices (CRN-4, QA-1). Each service will run in a separate container so it can scale independently (CONS-6, QA-2). Our tiers include the presentation/web app (UC-1), application/API tier (UC-4), and data tier (UC-6, QA-3). <br><br>We **rejected Non-Distributed Deployment** because it cannot support 5000+ AI users efficiently (QA-1, CRN-4, CONS-1) and all services would compete for resources (CRN-5, QA-4). |

<h2>Step 5: Elements, Responsibilities and Interfaces</h2>
The following table summarizes the design decisions made in the step.

| Design Decisions and Location | Rationale |
|-------------------------------|-----------|
| Remove local data storage from the Web Application client | Local storage is unnecessary because correctness and integration (CRN-1) require always using server-side data. |
| Add an API Gateway as the entry point for all client requests | The API Gateway simplifies request routing and centralizes authentication (UC-5), while improving usability. |
| Add an initial AI Query Service to handle natural language requests | AI queries are the core functionality (UC-4), so this is the first service to introduce. It improves efficiency (QA-4) and scalability (CONS-6). |

The results of these decisions have been recorded in the next step. Further iterations will define functionality in more detail, and interfaces will begin to be defined.

<h2>Step 6: Sketch and Record</h2>
The diagram below illustrates a sketch of the selected reference architectures chosen for the client and server applications.<br>
<img width="560" height="911" alt="image" src="https://github.com/user-attachments/assets/d239332e-eeaf-48f4-8d64-fccb99c80477" /><br>
We have provided descriptions of each element in the following table

| Element                   | Responsibility |
|---------------------------|----------------|
| Web App (UI)             | Displays dashboards, query interface, and notifications. Sends user actions to the UI Process module. |
| Client Session           | Stores the user’s session (SSO tokens and UI preferences). |
| Client Communication Module | Consumes backend services through standard HTTP calls. |
| Client Security          | Handles client-side validation and basic checks before sending requests. |
| API Gateway              | Entry point for all client requests. Routes each request to the correct backend service. |
| AI Query                 | Processes user questions and interacts with AI models to return personalized results. Main handler for UC-4. |
| Authentication           | Validates SSO tokens, manages login sessions, and enforces access control. |
| Dashboard Logic          | Generates personalized dashboards and academic or administrative data based on user role. |
| Database Access          | Handles all reads and writes to the AIDAP database. |
| External System Access   | Manages secure communication with LMS, Registration, and Scheduling systems. |
| Security                 | Handles server-side security, access validation, and enforcement across the API Gateway and Authentication Service. |
| Monitoring               | Monitors service health, logs failures, and tracks performance for API Gateway and backend services. |

The deployment diagram below illustrates where the components associated with the previous diagram will be deployed.<br>
<img width="1281" height="692" alt="image" src="https://github.com/user-attachments/assets/4dfe7152-ac73-4f41-9ed0-fd7e46631fa5" /> <br>

The following table summarizes the elements:

| Element         | Responsibility |
|-----------------|----------------|
| User Device     | Hosts the web browser that runs the Web App UI. Handles user input and displays dashboards and query responses. |
| Application Server | Hosts all server-side logic, including the API Gateway, AI Query Service, Authentication, and Dashboard Logic. |
| Database Server | Stores all core AIDAP data, including sessions, logs, dashboard configurations, and AI query records. |
| External Systems | University systems such as LMS, Registration, and Email that provide data to AIDAP through secure APIs. |

The table below summarizes the relationships between the aforementioned elements.

| Relationship                               | Description |
|--------------------------------------------|-------------|
| Between User Device and Application Server | Communication is performed through HTTPS. Client requests are routed to the API Gateway. |
| Between Application Server and Database    | The server communicates with the database using SQL. |
| Between Application Server and External Systems | Communication occurs through REST APIs. |

<h2>Step 7: Analysis and Review</h2>
The final step of this iteration is to summarize our design progress using the following Kanban board.

| Not Addressed | Partially Addressed | Completely Addressed | Design Decisions Made During the Iteration |
|---------------|----------------------|------------------------|--------------------------------------------|
|               | UC-1                 |                        | Dashboard Logic module defined, UI layer established. No full functionality yet. |
| UC-2          |                      |                        | No decisions have been made regarding content publishing. |
| UC-3          |                      |                        | Notification Service not created. |
|               | UC-4                 |                        | AI Query module and API Gateway defined. No full flow implemented yet. |
|               | UC-5                 |                        | Authentication defined, and SSO enforced through the gateway. Not fully designed. |
|               | UC-6                 |                        | External system access module defined. No import/export workflows defined. |
| UC-7          |                      |                        | No backup/restore defined. |
|               | QA-1                 |                        | Distributed deployment and monitoring modules defined. No recovery decisions made. |
|               | QA-2                 |                        | Microservices chosen and modular layers established. No CI/CD decisions. |
|               |                      | QA-3                   | Web App, microservices, and distributed deployment fully support portability. |
|               | QA-4                 |                        | API Gateway and AI Service modules defined. Caching not implemented. |
|               | QA-5                 |                        | Authentication and Security modules defined. Not fully designed. |
|               | CONS-1               |                        | AI Service and API Gateway chosen for efficiency. Optimization has not been done. |
|               | CONS-2               |                        | Distributed deployment supports uptime. No backup implemented. |
|               | CONS-3               |                        | Authentication and Gateway planned. SSO details unfinished. |
| CONS-4        |                      |                        | No encryption, privacy, or compliance methods defined. |
|               | CONS-5               |                        | Database access module defined, but no backup system. |
|               | CONS-6               |                        | Microservices architecture supports AI scaling. Update process not addressed. |
|               |                      | CRN-1                  | External system access defined. |
| CRN-2         |                      |                        | Security module exists, but privacy, encryption, and policies not addressed. |
| CRN-3         |                      |                        | Version management unaddressed. |
|               | CRN-4                |                        | Distributed deployment and microservices support scalability. No strategy defined. |
|               | CRN-5                |                        | Gateway and AI Service modules help. No caching or optimization. |
| CRN-6         |                      |                        | No context storage module. |
|               |                      | CRN-7                  | Microservices and distributed architecture fully support extensibility. |
|               | CRN-8                |                        | Monitoring module defined. No recovery logic. |
| CRN-3 | Version management unaddressed. |  |  |  |
| CRN-4 |  | Distributed deployment and microservices support scalability. No strategy defined. |  |  |
| CRN-5 |  | Gateway and AI Service modules help. No caching or optimization. |  |  |
| CRN-6 | No context storage module. |  |  |  |
| CRN-7 |  |  | Microservices and distributed architecture fully support extensibility. |  |
| CRN-8 |  | Monitoring module defined. No recovery logic. |  |  |
