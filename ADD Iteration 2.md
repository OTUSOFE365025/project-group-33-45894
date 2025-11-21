<h1>Iteration 2</h1>
<h3>(meeting the primary functionalities)</h3><br>


<h2>Step 2: Establish Iteration Goal</h2>
In this second iteration in the design of the AIDAP greenfield system, the goal is to identify the architecture in order to support functionality, the architects achieve this by considering the primary use cases:<br>
<br>
<ul>
  <li>UC-1 - Edit and View Personalized Dashboards</li>
  <li>UC-4 - Send Queries</li>
  <li>UC-5 - Manage Access</li>
</ul>


<h2>Step 3: Decompose</h2>
For this iteration, we refine the specific components that enable the primary use cases:<br>
<br>
<ul>
  <li>The Dashboard Logic Service for UC-1</li>
  <li>The AI Query Service for UC-4</li>
  <li>The Authentication and Access-Control Service for UC-5</li>
</ul>
<br>
We also refine the API Gateway, since all three use cases rely on it, and define the domain model that these services share. These elements were chosen because they directly implement the functional behaviour required in this phase, and because their internal structure must be clarified before defining interfaces, responsibilities, and data flows in later steps.


<h2>Step 4: Choose Design Concept(s) </h2>
<h3>(satisfy the inputs considered in the iteration)</h3>

| Design Decisions and Location | Rationale |
|---------|----------|
| **Create a domain model for AIDAP** | A domain model is required to support UC-1, UC-4, and UC-5. It identifies core objects (User, Session, Dashboard, Widget, QueryRequest, QueryResult) and provides the structure needed for functional decomposition. <br><br> Mapping UC-1, UC-4, and UC-5 to specific domain objects ensures each functional requirement has a clear architectural representation. <br><br> Separating responsibilities across layers maintains consistency with Iteration 1 and ensures proper handling of dashboards, queries, and access control. |
| **Define service APIs aligned with domain components** | Dashboard, Query, and Authentication APIs support UC-1, UC-4, and UC-5 through modular interfaces and centralized routing. |
| **Continue using cloud-native microservices from Iteration 1** | Independent services allow AI queries, dashboards, and authentication to scale separately. |
| **Use REST/JSON communication through the API Gateway** | REST provides simple, standard communication for all three use cases while centralizing authentication and routing. |


<h2>Step 5: Elements, Responsibilities and Interfaces</h2>
<h3>(instantiate architectural elemtents, allocate responsiblities and define interfaces)</h3>

| Design Decisions and Location | Rationale |
|---------|----------|
| **Create the Initial AIDAP Domain Model (User, Session, Dashboard, Widget, QueryRequest, QueryResult)** | Provides a stable abstraction layer for UC-1, UC-4, UC-5. Required for domain definition and iteration 3 extension. |
| **Map UC-1 to Domain Objects** | Dashboard → DashboardService; Widget → WidgetBuilder. Ensures that dashboard state and personalization are handled consistently. |
| **Map UC-4 to Domain Objects** | QueryRequest → AI Query Service; QueryResult → Presentation and Dashboard modules. Supports natural language functionality (R5). |
| **Map UC-5 to Domain Objects** | User → AuthenticationService; Session → TokenManager, Role → AuthorizationHandler. Provides secure SSO-linked access control. |
| **Decompose Domain Objects Across Layers** | Client handles display, Business Layer handles dashboard logic, AI microservice processes queries, Data Layer stores user dashboards, preferences, and logs. |
| **Define API Interfaces for Each Service** | Required to implement the selected domain responsibilities. Ensures loose coupling and service autonomy. |
| **Expose Public Methods via API Gateway** | Enforces centralized authentication/authorization before routing. |

<h2>Step 6: Sketch and Record</h2>
<h3>(sketch views and record design decisions)</h3>
<img width="1068" height="614" alt="Screenshot 2025-11-20 180854" src="https://github.com/user-attachments/assets/63acab08-cb02-459a-941b-1e76e2e345c0" />
<img width="838" height="651" alt="Screenshot 2025-11-20 174403" src="https://github.com/user-attachments/assets/e91a66fd-e4d0-4c7e-88c0-98a1b95a9b05" />
<img width="731" height="678" alt="Screenshot 2025-11-20 210747" src="https://github.com/user-attachments/assets/7f66c910-371c-42ab-9368-762f82f273c5" />

| Elements | Responsibility |
|---------|----------|
| AIDAP Web App (UI) | Presents dashboards, query interface, and admin screens to the user. Sends user actions (view dashboard, submit query, manage access) to the API Gateway. |
| Client | Represents the user-side environment that runs the AIDAP Web App UI. Sends all interactions to the server over HTTPS. |
| API Gateway | Serves as the single entry point for all AIDAP client traffic. Performs initial token checks and routes requests to Dashboard Logic Service, AI Query Service, or Auth & Access-Control Service. Provides centralized cross-cutting concerns. |
| Dashboard Logic Service | Implements UC-1. Loads and updates personalized dashboards using domain objects (Dashboard, Widget). Retrieves institution-related data through External Systems for dashboard display. |
| AI Query Service | Implements UC-4. Handles query submission, AI model invocation, and result retrieval. Manages QueryRequest and QueryResult lifecycle. Returns natural-language responses for dashboards or direct viewing. |
| Auth & Access-Control Service | Implements UC-5. Validates sessions, manages authentication, updates user roles, and enforces AccessRules. Ensures only authorized users can access protected AIDAP features. |
| AIDAP Data Store | Centralized storage for domain objects used by all services (User, Session, Dashboard, Widget, QueryRequest, QueryResult, AccessRule). Provides persistent data for dashboards, queries, and access control. |
| Service Agents | Adapters that handle communication between AIDAP and external university systems (LMS, Registration, Email) and AI providers. Translate internal requests into external API calls and manage error handling and protocol details. |
| Server | Hosts all backend logic and data services for AIDAP. Enforces security, executes business rules, and integrates with institutional systems and the database. |
| Business Layer | Contains all core application logic that implements the primary use cases. Coordinates dashboard rendering, AI query processing, and authentication/authorization before accessing data or external systems. |
| Data Layer | Encapsulates data access concerns for AIDAP. Provides a clear boundary between business logic and persistent storage or external data sources (university systems and AI providers). | 
| Other University Systems | External institutional systems such as LMS, Registration, and Email. Provide academic and administrative data that can be surfaced on dashboards or used as context for AI queries through Service Agents. |
| Database | Physical database platform that backs the AIDAP Data Store. Provides durable storage, indexing, and transactional guarantees for AIDAP data. |


<h1> Sequence Diagram for UC-1 Edit and View Personalized Dashboards </h1>
<img width="1477" height="494" alt="Screenshot 2025-11-20 220928" src="https://github.com/user-attachments/assets/e7cf78f3-0782-4380-8541-cb7e0601a3cb" />

| Element | Method Name | Description |
|---------|----------|----------|
| AIDAP Web App (UI) | getDashboardInfo(userID) | Requests the personalized dashboard… |
| API Gateway | routeDashboardRequest() | Forwards request after validation |
| Dashboard Logic | loadDashboard(userID) | Loads dashboard configuration |

<h1> Sequence Diagram for UC-4 Send Query </h1>
<img width="1554" height="595" alt="Screenshot 2025-11-20 222548" src="https://github.com/user-attachments/assets/196b9214-4cba-4749-a645-e82dd61f372a" />

| Element | Method Name | Description |
|---------|----------|----------|
| AIDAP Web App (UI) | sendQuery(queryText) | Sends the user's natural-language query to AIDAP. |
| AIDAP Web App (UI) | getQueryResult(queryID) | Requests the processed AI answer for the submitted query. |
| API Gateway | routeQuerySubmission() | Routes the query submission to the AI Query Service. |
| API Gateway | routeQueryResultRequest() | Forwards result-polling requests to the AI Query Service. |
| AI Query Service | createQuery(queryText) | Creates a new QueryRequest object and assigns an ID. |
| AI Query Service | processQuery(queryID) | Runs AI inference and stores the resulting QueryResult. |
| AI Query Service | getQueryResult(queryID) | Returns the current status or final result of the query. |
| AIDAP Query Database | storeQuery(queryID) | Stores QueryRequest and QueryResult records. |
| AIDAP Query Database | loadQuery(queryID) | Retrieves stored query data and result for the given ID. |
| External AI Provider | execute(prompt) | Performs the AI inference and returns a raw response. |

<h1> Sequence Diagram for UC - 5 </h1>
<img width="1418" height="616" alt="image" src="https://github.com/user-attachments/assets/8f0b4fdc-7eaf-45f5-ae21-65d9f1658e32" />

| Element | Method Name | Description |
|---------|----------|----------|
| AIDAP Admin UI (Web App) | changeUserRoles(userID) | Sends a request to update the roles assigned to a specific AIDAP user. |
| AIDAP Admin UI (Web App) | changeAccessRule(ruleID) | Sends a request to modify an existing AccessRule. |
| API Gateway | routeRoleUpdateRequest() | Routes user-role update requests to the Auth & Access-Control Service. |
| API Gateway | routeAccessRuleUpdateRequest() | Forwards access-rule modification requests to the Auth & Access-Control Service. |
| Auth & Access-Control Service | changeUserRoles(userID) | Applies updates to the user's role assignments. |
| Auth & Access-Control Service | changeAccessRule(ruleID) | Applies updates to the specified access rule. |
| Auth & Access-Control Service | validateAdmin(token) | Ensures the caller has admin privileges before updating roles or rules. |
| AIDAP Auth Database | updateUserRecord(userID) | Stores revised role information for the user. |
| AIDAP Auth Database | updateAccessRule(ruleID) | Stores the updated access rule definition. |
| Admin Notification Component | notifyAdmin() | Notifies the admin that the update was applied successfully. |




<h2>Step 7: Analysis and Review</h2>

| Not Addressed | Partially Addressed | Completely Addressed | Design Decisions Made During the Iteration |
|---------|----------|----------|----------|
|  | UC-1 |  | Refined Dashboard Logic Service and mapping of Dashboard > DashboardService and Widget > WidgetBuilder. Domain model supports dashboard personalization. |
|  | UC-4 |  | AI Query Service defined with QueryRequest and QueryResult domain objects. Public methods exposed through API Gateway. |
|  | UC-5 |  | Authentication & Access-Control refined: User, Session, Role mapped to AuthenticationService, TokenManager, AuthorizationHandler. |
| | QA-1 |  | Reliability supported through centralized API Gateway, monitoring, and service decomposition. |
|  | QA-4 |  | Efficiency improved by separating AI Query Service and Dashboard Logic Service; REST/JSON ensures lightweight communication. |
|  |  | QA-3 | Web App + cloud-native microservices architecture supports portability across environments. |
|  | QA-5 |  | Access rules, token validation, and authorization integrated into Auth Service. |
|  | CONS-1 | | Latency-sensitive operations centralized through Gateway; routing minimized. |
| CONS-2 |  |  | Failover/redundancy strategies not yet designed in this iteration. |
|  | CONS-3 |  | SSO token validation and session handling partially implemented. |
|  | CONS-6 |  | AI Query scaling supported by microservice separation; model update strategy to be addressed in next iteration. |
|  | CRN-1 |  | LMS/Registration/Email integration points defined in Dashboard Logic Service. |
|  | CRN-4 |  | Scalability supported through microservice decomposition; capacity planning deferred. |
| CRN-6 |  |  | Context memory/personalization store not yet introduced (Iteration 3). |

