# Defense Presentation — LFU Project (Levee Fair Use)

> **Real-time data stream processing with Apache Flink & Kubernetes**
> Final-year project — Sofrecom (Orange Group) — ESPRIT

---

## Structure overview

| # | Section | Slides | Indicative duration |
|---|---------|--------|---------------------|
| 0 | Title page | 1 | 30 s |
| 1 | Presentation outline | 1 | 30 s |
| 2 | Introduction & Host organization | 3 | 2 min |
| 3 | Study of the existing system & Problem | 3 | 2.5 min |
| 4 | Proposed solution | 2 | 1.5 min |
| 5 | Requirements analysis | 2 | 1.5 min |
| 6 | Solution architecture | 4 | 3.5 min |
| 7 | Implementation (6 Sprints) | 7 | 5 min |
| 8 | **DEMO** | 4 | 4 min |
| 9 | Monitoring & Observability | 2 | 1.5 min |
| 10 | Assessment, conclusion & perspectives | 2 | 1.5 min |
| 11 | Acknowledgments / Questions | 1 | — |

**Target total duration: ~18–20 minutes** (excluding questions). A final-year project defense usually lasts 15–20 min of presentation + jury questions.

---

## SECTION 0 — Title page

### Slide 1 — Title
**Content to display:**
- Logos: ESPRIT + Sofrecom / Orange
- Project title: *"Setting up a real-time data stream processing system for Push notifications"*
- Subtitle: *LFU Project — Levee Fair Use*
- Your name, academic supervisor, company supervisor
- Academic year

**Speech:**
> "Good morning, Mr. President of the jury, ladies and gentlemen of the jury. Thank you for your presence. My name is [Name] and I will present my final-year project, carried out at Sofrecom, a subsidiary of the Orange Group. This project, called LFU for *Levee Fair Use*, focuses on setting up a real-time data stream processing system for sending push notifications."

---

## SECTION 1 — Outline

### Slide 2 — Presentation outline
**Content:** List of main sections (Introduction, Problem, Solution, Architecture, Implementation, Demo, Conclusion).

**Speech:**
> "I will begin by presenting the project context and the host organization, then I will analyze the existing system and its shortcomings. I will then walk you through the proposed solution and its technical architecture, before detailing the implementation across six sprints. A demonstration will be presented, followed by the monitoring aspect, and I will conclude with a summary and the future perspectives."

---

## SECTION 2 — Introduction & Host organization

### Slide 3 — General context
**Content:**
- Visual keywords: IoT, Big Data, Streaming, real-time
- Idea: explosion of data volumes → need for instant responsiveness
- Apache Flink as a stream processing platform

**Speech:**
> "Today, with the rise of the Internet of Things and streaming applications, real-time data processing has become a major challenge. Companies seek to react instantly to events in order to improve their services. It is in this context that my project takes place, exploring the use of Apache Flink, one of the leading stream processing platforms, offering horizontal scalability and low latency."

### Slide 4 — Host organization
**Content:**
- Orange Group: key figures (€42.3 B revenue, 259 M customers, 26 countries)
- Sofrecom Tunisia: subsidiary for consulting and digital transformation of telecoms
- Departments: software development, architecture, information systems security

**Speech:**
> "This project was carried out at Sofrecom, a subsidiary of the Orange Group. Orange is one of the world's leading telecommunications operators, with 259 million customers across 26 countries. Sofrecom supports the digital transformation of telecom players. I joined the software development department as a Java backend developer."

### Slide 5 — Project scope: the LFU project
**Content:**
- Objective: notify customers in real time when they reach their data consumption threshold
- "Fair Use" = fair usage / consumption threshold

**Speech:**
> "The LFU project aims to set up a real-time data processing system dedicated to sending push notifications. These notifications allow Orange customers to be informed instantly when they reach their data consumption threshold."

---

## SECTION 3 — Study of the existing system & Problem

### Slide 6 — Existing system (1/2): the Java daemon
**Content:**
- Diagram: `img/demon java.png`
- Step 1: Java daemon reads RabbitMQ → transforms → stores in database

**Speech:**
> "The current system works in two steps. First, a Java daemon runs continuously to retrieve messages from RabbitMQ, perform the necessary transformations, and then store them in the database."

### Slide 7 — Existing system (2/2): the batch
**Content:**
- Diagram: `img/batch job.png`
- Step 2: a batch runs twice a day → sends to the Erable API → notifies customers

**Speech:**
> "Second, a batch process is scheduled to run twice a day to process the stored messages and send them to the Erable API, which then notifies the customers."

### Slide 8 — Critique of the existing system
**Content (list of weaknesses):**
- ⏱️ Notification delay (periodic execution)
- 📈 Inability to handle load spikes
- 🐛 Late anomaly detection
- 🧩 Architecture complexity (batch scheduling)
- 💸 Resource waste
- 🔧 Heavy maintenance (two-part architecture)

**Speech:**
> "After analysis, we identified several limitations: first, a notification delay due to the periodic execution of batch processes; then a difficulty absorbing load spikes; late anomaly detection; a complex architecture to schedule; resource waste, since batches are triggered even without data; and finally costly maintenance linked to this two-part architecture."

---

## SECTION 4 — Proposed solution

### Slide 9 — Target solution
**Content:**
- Diagram: `img/Solution.png`
- RabbitMQ → Flink (real-time processing) → Erable → Customer
- Key points: real-time, auto-scaling, fast detection, monitoring, AI module

**Speech:**
> "Our solution consists of setting up a real-time stream processing system. It connects directly to RabbitMQ as a source, processes events on the fly, then sends them to the Erable API for immediate delivery to customers. In case of increased load, processing instances are launched automatically. We also integrate a supervision block and an artificial intelligence module capable of predicting a quota overshoot based on consumption patterns."

### Slide 10 — Scrum methodology
**Content:**
- Diagram: `img/Scrum_Framework.png`
- Roles: Scrum Master, Product Owner, development team
- 6 sprints of 4 weeks

**Speech:**
> "The project was carried out following the agile Scrum methodology, with four-week iterations. The work was divided into six sprints, each delivering a functional increment of the system, from the initial project setup through to full observability in production."

---

## SECTION 5 — Requirements analysis

### Slide 11 — Functional requirements & actor
**Content:**
- **Actor:** the administrator (Flink job management, monitoring)
- **Functional requirements:** continuous ingestion from RabbitMQ, real-time processing, enrichment, AI prediction, dispatch, monitoring
- Use case diagram: `img/Global Use Case.drawio.png`

**Speech:**
> "The system has a single actor: the administrator, who manages the Flink jobs and monitors logs and metrics. The main functional requirements are: continuous ingestion of messages from RabbitMQ, their real-time processing, their enrichment with external data, the prediction of quota overshoot using machine learning, notification dispatching, and the implementation of supervision tools."

### Slide 12 — Non-functional requirements
**Content (icons + keywords):**
- 🟢 High availability
- 🛡️ Fault tolerance
- 📊 Scalability
- ⚡ Reliability / low latency
- 🔒 Security

**Speech:**
> "On the non-functional side, the system must guarantee high availability and fault tolerance through automatic recovery mechanisms, scalability adapted to the load, reliability with minimal latency, and finally the security of access and data."

---

## SECTION 6 — Solution architecture

### Slide 13 — Apache Flink: overview
**Content:**
- Diagram: `img/flink_technical_archi.png`
- Open-source framework for distributed processing of massive streams
- Master/Slave mode: JobManager (orchestrator) + TaskManagers (workers)

**Speech:**
> "Apache Flink is an open-source framework for distributed processing of massive data. It works in master-slave mode: the JobManager orchestrates and distributes the tasks, while the TaskManagers execute them. Its scalable execution engine enables stream processing with low latency."

### Slide 14 — Fault tolerance: Checkpointing & High availability
**Content:**
- Diagram: `img/checkpointing.png` and/or `img/ha_jobmanager.png`
- Checkpointing: regular snapshots of the state → recovery after failure
- High availability: leader JobManager + standby (leader election via Kubernetes)

**Speech:**
> "One of Flink's strengths is its checkpointing mechanism: regular snapshots of the application state allow, in case of failure, to resume processing from the last savepoint. To avoid the single point of failure represented by the JobManager, we use a high-availability mechanism with leader election, handled here by Kubernetes."

### Slide 15 — Kubernetes
**Content:**
- Diagram: `img/k8s archi.jpg`
- Container orchestrator: deployment, scaling, self-healing
- Master (control plane) + Worker nodes
- Flink/K8S integration: horizontal scaling, resource management

**Speech:**
> "For deployment, we use Kubernetes, a container orchestrator that automates the deployment, scaling, and management of containerized applications. Its native integration with Flink allows us to benefit from horizontal scaling and optimal resource management: TaskManagers are created or removed on the fly according to the load."

### Slide 16 — Global architecture of the solution
**Content:**
- Diagram: `img/global architecture.png`
- Components: JobManager, TaskManager, RabbitMQ, external APIs (Valkey, Erable), API Gateway, ONNX Runtime, S3 (checkpoints), Prometheus/Grafana, ELK

**Speech:**
> "Here is the global architecture of our solution. The LFU job is deployed on a dedicated Flink cluster, in *per-job* mode. It consumes messages from RabbitMQ via AMQPS, interacts with two external APIs — Valkey for subscriptions and Erable for sending notifications — all through a secure API Gateway. The AI model inference runs directly within the pipeline thanks to ONNX Runtime. The state is saved in an S3 bucket, and supervision is provided by Prometheus, Grafana, and the ELK stack."

---

## SECTION 7 — Implementation (6 Sprints)

> **Tip:** show a "sprint timeline" slide then one slide per sprint. Emphasize the pipeline being built progressively.

### Slide 17 — Sprints overview
**Content:** planning table (theme + deliverables per sprint).

| Sprint | Theme | Key deliverable |
|--------|-------|-----------------|
| 1 | Setup & Ingestion | Maven structure, RabbitMQ source |
| 2 | Validation & Subscription | Message validation, Valkey integration |
| 3 | Notification delivery | Erable API call, state update |
| 4 | AI module | ONNX inference, fallback |
| 5 | Containerization & Deployment | Docker, Kubernetes, CI/CD |
| 6 | Observability | Kibana, Grafana, alerting |

**Speech:**
> "The implementation took place over six sprints. Each one added a building block to the pipeline: ingestion, validation, delivery, artificial intelligence, deployment, then observability. I will present the highlights of each one."

### Slide 18 — Sprint 1: Setup & Message ingestion
**Content:**
- Multi-module Maven structure (`pushavoo-flink-core`, `job-fair-use`, …)
- Technology stack: Java 17, Flink, Kubernetes, Docker, ONNX…
- RabbitMQ source operator (AMQPS) — `img/RabbitMQ source.png`
- Job workflow: `img/job_workflow.png`

**Speech:**
> "The first sprint laid the foundations: a multi-module Maven structure promoting component reuse, the environment configuration, and the implementation of the first pipeline operator — the RabbitMQ source, which consumes JSON messages via the secure AMQPS protocol."

### Slide 19 — Sprint 2: Validation & Subscription
**Content:**
- Validation & deserialization (Jackson + Hibernate Validator, French MSISDN)
- Enrichment with a traceability UUID
- Subscription retrieval from Valkey + business eligibility rules

**Speech:**
> "The second sprint implemented the validation of incoming messages — JSON format, mandatory fields, valid MSISDN number — then the enrichment with subscription data retrieved from the Valkey API. Business rules then determine whether the customer is eligible for the notification based on their type, threshold, and history."

### Slide 20 — Sprint 3: Notification delivery
**Content:**
- Logical pipeline: `img/Diagramme seq.png` or the pipeline diagram
- Erable API call via WebClient + retry mechanism
- Sink: state update in Valkey
- Error handling (retry, critical errors → Flink restart)

**Speech:**
> "The third sprint completed the core of the pipeline: sending the notification to the Erable API via an HTTP call with an automatic retry mechanism, then updating the subscription state in Valkey. Fine-grained error handling distinguishes transient errors — which trigger a retry — from critical errors — which cause a controlled restart of the job."

### Slide 21 — Sprint 4: Artificial Intelligence module
**Content:**
- Objective: predict the quota overshoot probability + anomaly detection (threshold 0.85)
- 7 normalized features (reached threshold, customer type, hour, day, recency, billing cycle, event type)
- GBDT model (XGBoost) exported to ONNX, inference < 1 ms via ONNX Runtime
- Rule-based fallback if model unavailable (non-blocking pipeline)
- Results: `img/model training.png`

**Speech:**
> "The fourth sprint integrated an artificial intelligence layer. From seven normalized features, a Gradient Boosting model, exported to the ONNX format, predicts in real time the quota overshoot probability and detects consumption anomalies. The inference, under one millisecond, runs directly within the pipeline. Importantly, this module is non-blocking. If the model is unavailable, a rule-based fallback algorithm takes over, ensuring that the notification flow is never interrupted."

### Slide 22 — Sprint 5: Containerization & Deployment
**Content:**
- Docker image (fat JAR via maven-shade, embedded ONNX model)
- Kubernetes deployment with Helm (Job, Deployment, Services, ConfigMap)
- High availability: 2 active/passive JobManagers + ZooKeeper, S3 checkpoints
- CI/CD pipeline: `img/CICD pipeline.png` (Build → Fortify/Sonar analysis → DockerBuild)

**Speech:**
> "The fifth sprint packaged the job into a Docker image and deployed it on Kubernetes using Helm. The production architecture is highly available, with two JobManagers in active/passive mode coordinated by ZooKeeper, and checkpoints stored on S3. A continuous integration pipeline automates compilation, security and quality analysis with Fortify and Sonar, then the build and publication of the Docker image."

### Slide 23 — Sprint 6: Observability & Monitoring
**Content:**
- Structured JSON logs → ELK (Elasticsearch, Logstash, Kibana)
- Flink metrics → Prometheus → Grafana (4 dashboards)
- Alerting rules + end-to-end traceability via UUID

**Speech:**
> "Finally, the sixth sprint gave the system full observability. The logs, structured in JSON, are centralized in the ELK stack and explored via Kibana. The metrics are collected by Prometheus and visualized in four Grafana dashboards. Alerting rules proactively warn of any degradation, and each notification is traceable end-to-end thanks to a unique identifier."

---

## SECTION 8 — DEMONSTRATION 🎬

> **Demo tips:**
> - Prepare a **backup video** (screencast) in case the environment does not respond.
> - Have the interfaces already open in tabs (RabbitMQ, Flink UI, Kibana, Grafana).
> - Prepare a **test JSON message** ready to publish.
> - Announce out loud what the jury is about to see *before* showing it.

### Slide 24 — Demonstration scenario
**Content:** diagram of a message's journey + demo steps.
- **Step 1:** Publish a message in RabbitMQ
- **Step 2:** Observe the processing in the Flink UI
- **Step 3:** Check the logs in Kibana
- **Step 4:** Visualize the metrics in Grafana

**Speech:**
> "I will now present a demonstration of the system in action. We will follow the complete journey of a message: I will publish a consumption event in RabbitMQ, observe its real-time processing in the Flink interface, find its trace in the Kibana logs, and finally visualize its impact on the Grafana metrics."

### Slide 25 — Demo 1: Ingestion & processing (RabbitMQ + Flink UI)
**Content / screenshots:** `img/RabbitMQ source.png`, `img/flink_ui.png`, `img/flink_topology.png`
- Show the RabbitMQ queue (throughput, ready/unacked messages)
- Show the job topology and the RUNNING state in the Flink UI

**Speech:**
> "Here I publish a *fairuse* message for a customer reaching 85% of their quota. We immediately observe, in the RabbitMQ management interface, the message being consumed. In the Flink UI, the job topology shows the data flowing through the various operators: source, validation, subscription retrieval, AI prediction, Erable call, and state update. The job is indeed in the *Running* state."

### Slide 26 — Demo 2: Log traceability (Kibana)
**Content / screenshots:** `img/subscription found log.png`, `img/Erable problem log.png`
- Search by `uuid` or `msisdn`
- Show a success log (main record found, Erable send OK)
- Show an example of an error log (code 5030)

**Speech:**
> "In Kibana, I search for our message by its unique identifier. We find its full trace: the successful retrieval of the subscription, the prediction score, then the successful send to Erable. Each log is structured and enriched with fields such as customCode, MSISDN, or SID, allowing precise filtering. I can also, for example, isolate all Erable access errors via code 5030."

### Slide 27 — Demo 3: Metrics & Dashboards (Grafana)
**Content / screenshots:** `img/lfu grafana 1.png`, `img/lfu grafana 2.png`, `img/grafana for resources.png`, `img/grafana for job stats.png`
- Job health & QoS dashboard
- Event metrics dashboard (sent / discarded / errored)
- Resource and job statistics dashboard

**Speech:**
> "Finally, on Grafana, we visualize in real time the impact of our message: the counter of sent notifications increments. The dashboards cover the job health and its quality of service, the event metrics, the cluster resource consumption, and the flow statistics per operator. In case of an anomaly, an alerting rule would immediately notify the team. This concludes the demonstration."

---

## SECTION 9 — Monitoring & Results (summary)

### Slide 28 — Results & benefits
**Content:**
- ✅ Real-time notifications (vs. 2×/day previously)
- ✅ Automatic scalability according to the load
- ✅ Fault tolerance (checkpoints + HA)
- ✅ Predictive overshoot detection via AI
- ✅ End-to-end observability

**Speech:**
> "In terms of results, the solution transforms a twice-daily batch process into real-time processing. It brings automatic scalability, fault tolerance guaranteed by checkpoints and high availability, predictive intelligence thanks to the AI module, and complete observability of the system."

### Slide 29 — Key figures / model metrics
**Content:** AI model targets (Accuracy ≥ 90%, Precision ≥ 85%, Recall ≥ 80%, F1 ≥ 82%, AUC-ROC ≥ 0.92) + inference latency < 1 ms.

**Speech:**
> "The prediction model achieves high performance: satisfactory precision and recall, an AUC-ROC above 0.92, for an inference latency below one millisecond — perfectly compatible with stream processing."

---

## SECTION 10 — Conclusion & Perspectives

### Slide 30 — Conclusion
**Content:**
- Reminder of the achieved objective: real-time pipeline Flink + Kubernetes
- Personal assessment: putting theory into practice, skill growth (Flink, K8S, AI, DevOps)

**Speech:**
> "To conclude, this project allowed me to design and implement a complete real-time data processing pipeline, combining Apache Flink and Kubernetes, and integrating an AI layer directly into the stream. It was a very enriching experience, combining the theory acquired during my studies with concrete hands-on practice on cutting-edge technologies."

### Slide 31 — Perspectives
**Content:**
- 🔁 Adaptive retraining of the model on production data
- 🧪 Multi-model A/B testing
- 📈 Extension of horizontal scaling to other use cases
- 🔌 Integration of additional data sources

**Speech:**
> "Several avenues for improvement are possible: adaptive retraining of the model using production data to improve its accuracy, the implementation of A/B testing between several model versions, the extension of scaling to other use cases, and the integration of new data sources to enrich the predictions."

---

## SECTION 11 — Closing

### Slide 32 — Acknowledgments / Questions
**Content:** "Thank you for your attention — Questions?" + optional contact details.

**Speech:**
> "Thank you for your attention, and I remain at your disposal to answer any questions."

---

## Useful appendices (backup slides for jury questions)

Prepare a few "backup" slides (not presented but ready) to anticipate questions:

| Possible topic | Backup slide |
|----------------|--------------|
| Why Flink and not Spark/Storm? | Native streaming vs. micro-batch comparison |
| Data model | `FairUseMessage` / `EnrichedFairUseMessage` tables |
| Detailed sequence diagram | `img/Diagramme seq.png`, `img/Diag sec get sub.drawio.png` |
| Activity diagram | `img/Diag activity.drawio.png` |
| Security (OAuth2 / API Gateway) | `img/flink_connectivity.png` |
| Log codes | `CodeMessageLog` table |
| Delivery guarantees (at-least-once) | Checkpointing + correlation ID slide |

---

## Presentation tips

1. **The 1 slide / minute rule**: ~30 slides for ~20 min. Do not overload the slides (max 5–6 bullets).
2. **Favor diagrams**: your report contains excellent diagrams (`img/`), use them rather than text.
3. **Common thread**: always follow the journey of a message. This makes the presentation narrative and clear.
4. **Demo = strong point**: this is what the jury remembers. Rehearse it several times, time it, and prepare a backup video.
5. **Transitions**: end each section with a sentence that announces the next one.
6. **Figures**: striking figures (259 M customers, latency < 1 ms, 2×/day → real-time) leave a mark.
7. **Anticipate questions** on technical choices (Flink vs Spark, at-least-once vs exactly-once, ONNX, HA).

