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
| Method Name | Description |
|---------|----------|
| Element: AIDAP Web App (UI) |
| getDashboardInfo(userID) | Requests the personalized dashboard for the logged-in user. |
| Element: API Gateway |
| routeDashboardRequest() | Forwards the dashboard request to the Dashboard Logic Service after validating the token. |
| Element: Dashboard Logic |
| loadDashboard(userID) | Returns dashboard and widget configuration for the user. |

<h1> Sequence Diagram for UC-4 Send Query </h1>
<img width="1554" height="595" alt="Screenshot 2025-11-20 222548" src="https://github.com/user-attachments/assets/196b9214-4cba-4749-a645-e82dd61f372a" />
| Method Name | Description |
|---------|----------|
| Element: AIDAP Web App (UI) |
| sendQuery(queryText) | Sends the user’s natural language query to AIDAP. |
| getQueryResult(queryID) | Requests the processed AI answer. |
| Element: API Gateway |
| routeQuerySubmission() | Sends query submissions to the AI Query Service. |
| Element: AIDAP AI Query Service |
| createQuery(queryText) | Creates a new QueryRequest. |
| processQuery(queryID) | Runs AI inference and stores the result. | 
| getQueryResult(queryID) | Returns the current result or status. |
| Element: AIDAP Query Database |
| storeQuery(queryID) | Stores QueryRequest and QueryResult objects. |
| loadQuery(queryID) | Retrieves query status and result. |
| Element: External AI Provider |
| execute(prompt) | Executes AI inference and returns a raw response. |

<h1> Sequence Diagram for UC - 5 </h1>
<img width="1418" height="616" alt="image" src="https://github.com/user-attachments/assets/8f0b4fdc-7eaf-45f5-ae21-65d9f1658e32" />
| Method Name | Description |
|---------|----------|
| Element: AIDAP Admin UI |
| changeUserRoles(userID) | Sends a request to update the roles of an AIDAP user. |
| changeAccessRule(ruleID) | Sends a request to update access rule definitions. |
| Element: API Gateway |
| routeAdminRequest() | Forwards admin access requests to the Auth Service. |
| Element: Auth & Access-Control Service |
| changeUserRoles(userID) | Updates user roles in the auth store. |
| changeAccessRule(ruleID) | Updates access rule settings. |
| Element: AIDAP Auth Database |
| updateUserRecord(userID) | Stores updated roles. |
| updateAccessRule(ruleID) | Stores updated access rules. |




*NOTE include deployment diagram<br>
**NOTE include domain specific models<br>
<br>

<h2>Step 7: Analysis and Review</h2>
<h3>(perform analysis of current design and review iteration goal + design objectives)</h3><br>
