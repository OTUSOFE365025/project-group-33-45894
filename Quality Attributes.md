| ID   | Quality Attribute | Scenario | Associated Use Case |
|:-----|:---|:---| :---|
|QA-1 | Reliability | The system maintains a 99.5% uptime per month and continues to operate correctly under both normal and peak loads (≤ 5,000 concurrent users). During outages, services recover automatically within 30 minutes. | UC-1, UC-4, UC-7 |
|QA-2 | Maintainability | Updates, such as AI model retraining or dashboard feature upgrades, are deployed via CI/CD pipelines with no downtime, reflecting the ease of software updates through a modular design. | UC-1, UC-4, UC-7 |
|QA-3 | Usability | Dashboards, notifications, and query interfaces are intuitive and accessible, supporting responsive layouts, voice input, and consistent UI/UX across all platforms. | UC-1, UC-3, UC-4 |
|QA-4 | Portability | The software can be migrated or deployed across cloud environments and institutional infrastructures without code changes, ensuring compatibility with standard OS and container platforms. | UC-1, UC-6 |
|QA-5 | Correctness | The system accurately reflects academic and administrative data. Query and dashboard results match underlying institutional databases with verified precision. | UC-1, UC-2, UC-4 |
|QA-6 | Efficiency | All pages and data requests load in ≤ 2 seconds under normal conditions through optimized caching, AI inference, and resource management. | UC-1, UC-3, UC-4 |
|QA-7 | Security | All user interactions are protected via SSO authentication, encrypted data storage, and adherence to privacy compliance policies to prevent unauthorized access. | UC-2, UC-4, UC-5 |
|QA-8 | Reusability | Core modules (authentication, notifications, dashboards) are built as reusable components shared across AIDAP services to reduce redevelopment effort. | UC-1, UC-3, UC-5 |
