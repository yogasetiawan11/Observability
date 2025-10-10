# What is Jaeger
Jaeger is an open-source, end-to-end distributed tracing system used for monitoring and troubleshooting microservices-based architectures. It helps developers understand how requests flow through a complex system, by tracing the path a request takes and measuring how long each step in that path takes.

# Why use jaeger
In modern applications, especially microservices architectures, a single user request can touch multiple services. When something goes wrong, it’s challenging to pinpoint the source of the problem. Jaeger helps by:

- Identifying bottlenecks: See where your application spends most of its time.

- Finding root causes of errors: Trace errors back to their source.

- Optimizing performance: Understand and improve the latency of services.

# Component of Jaeger
Jaeger consists of several components:
- Agent: Collects traces from your application only if developer has instrumented tracing in their app.
- Collector: Receives traces from the agent and processes them.
- Storage/DB: Stores traces for later retrieval (often a database like Elasticsearch) so the collector information can be store in DB. and you need to configure it .
- UI Query: Provides a UI to view traces. In UI you fire a query to the collector It gets the information trough the elastic search DB and display it to the users.
