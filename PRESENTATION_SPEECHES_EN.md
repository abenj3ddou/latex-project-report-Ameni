# LFU Defense Speeches (English)

This document copies all speeches from `PRESENTATION_STRUCTURE_EN.md`, grouped by slide number.

## Slide 1
Good morning, Mr. President of the jury, ladies and gentlemen of the jury. Thank you for your presence. My name is [Name] and I will present my final-year project, carried out at Sofrecom, a subsidiary of the Orange Group. This project, called LFU for *Levee Fair Use*, focuses on setting up a real-time data stream processing system for sending push notifications.

## Slide 2
I will begin by presenting the project context and the host organization, then I will analyze the existing system and its shortcomings. I will then walk you through the proposed solution and its technical architecture, before detailing the implementation across six sprints. A demonstration will be presented, followed by the monitoring aspect, and I will conclude with a summary and the future perspectives.

## Slide 3
Today, with the rise of the Internet of Things and streaming applications, real-time data processing has become a major challenge. Companies seek to react instantly to events in order to improve their services. It is in this context that my project takes place, exploring the use of Apache Flink, one of the leading stream processing platforms, offering horizontal scalability and low latency.

## Slide 4
This project was carried out at Sofrecom, a subsidiary of the Orange Group. Orange is one of the world's leading telecommunications operators, with 259 million customers across 26 countries. Sofrecom supports the digital transformation of telecom players. I joined the software development department as a Java backend developer.

## Slide 5
The LFU project aims to set up a real-time data processing system dedicated to sending push notifications. These notifications allow Orange customers to be informed instantly when they reach their data consumption threshold.

## Slide 6
The current system works in two steps. First, a Java daemon runs continuously to retrieve messages from RabbitMQ, perform the necessary transformations, and then store them in the database.

## Slide 7
Second, a batch process is scheduled to run twice a day to process the stored messages and send them to the Erable API, which then notifies the customers.

## Slide 8
After analysis, we identified several limitations: first, a notification delay due to the periodic execution of batch processes; then a difficulty absorbing load spikes; late anomaly detection; a complex architecture to schedule; resource waste, since batches are triggered even without data; and finally costly maintenance linked to this two-part architecture.

## Slide 9
Our solution consists of setting up a real-time stream processing system. It connects directly to RabbitMQ as a source, processes events on the fly, then sends them to the Erable API for immediate delivery to customers. In case of increased load, processing instances are launched automatically. We also integrate a supervision block and an artificial intelligence module capable of predicting a quota overshoot based on consumption patterns.

## Slide 10
The project was carried out following the agile Scrum methodology, with four-week iterations. The work was divided into six sprints, each delivering a functional increment of the system, from the initial project setup through to full observability in production.

## Slide 11
The system has a single actor: the administrator, who manages the Flink jobs and monitors logs and metrics. The main functional requirements are: continuous ingestion of messages from RabbitMQ, their real-time processing, their enrichment with external data, the prediction of quota overshoot using machine learning, notification dispatching, and the implementation of supervision tools.

## Slide 12
On the non-functional side, the system must guarantee high availability and fault tolerance through automatic recovery mechanisms, scalability adapted to the load, reliability with minimal latency, and finally the security of access and data.

## Slide 13
Apache Flink is an open-source framework for distributed processing of massive data. It works in master-slave mode: the JobManager orchestrates and distributes the tasks, while the TaskManagers execute them. Its scalable execution engine enables stream processing with low latency.

## Slide 14
One of Flink's strengths is its checkpointing mechanism: regular snapshots of the application state allow, in case of failure, to resume processing from the last savepoint. To avoid the single point of failure represented by the JobManager, we use a high-availability mechanism with leader election, handled here by Kubernetes.

## Slide 15
For deployment, we use Kubernetes, a container orchestrator that automates the deployment, scaling, and management of containerized applications. Its native integration with Flink allows us to benefit from horizontal scaling and optimal resource management: TaskManagers are created or removed on the fly according to the load.

## Slide 16
Here is the global architecture of our solution. The LFU job is deployed on a dedicated Flink cluster, in *per-job* mode. It consumes messages from RabbitMQ via AMQPS, interacts with two external APIs — Valkey for subscriptions and Erable for sending notifications — all through a secure API Gateway. The AI model inference runs directly within the pipeline thanks to ONNX Runtime. The state is saved in an S3 bucket, and supervision is provided by Prometheus, Grafana, and the ELK stack.

## Slide 17
The implementation took place over six sprints. Each one added a building block to the pipeline: ingestion, validation, delivery, artificial intelligence, deployment, then observability. I will present the highlights of each one.

## Slide 18
The first sprint laid the foundations: a multi-module Maven structure promoting component reuse, the environment configuration, and the implementation of the first pipeline operator — the RabbitMQ source, which consumes JSON messages via the secure AMQPS protocol.

## Slide 19
The second sprint implemented the validation of incoming messages — JSON format, mandatory fields, valid MSISDN number — then the enrichment with subscription data retrieved from the Valkey API. Business rules then determine whether the customer is eligible for the notification based on their type, threshold, and history.

## Slide 20
The third sprint completed the core of the pipeline: sending the notification to the Erable API via an HTTP call with an automatic retry mechanism, then updating the subscription state in Valkey. Fine-grained error handling distinguishes transient errors — which trigger a retry — from critical errors — which cause a controlled restart of the job.

## Slide 21
The fourth sprint integrated an artificial intelligence layer. From seven normalized features, a Gradient Boosting model, exported to the ONNX format, predicts in real time the quota overshoot probability and detects consumption anomalies. The inference, under one millisecond, runs directly within the pipeline. Importantly, this module is non-blocking. If the model is unavailable, a rule-based fallback algorithm takes over, ensuring that the notification flow is never interrupted.

## Slide 22
The fifth sprint packaged the job into a Docker image and deployed it on Kubernetes using Helm. The production architecture is highly available, with two JobManagers in active/passive mode coordinated by ZooKeeper, and checkpoints stored on S3. A continuous integration pipeline automates compilation, security and quality analysis with Fortify and Sonar, then the build and publication of the Docker image.

## Slide 23
Finally, the sixth sprint gave the system full observability. The logs, structured in JSON, are centralized in the ELK stack and explored via Kibana. The metrics are collected by Prometheus and visualized in four Grafana dashboards. Alerting rules proactively warn of any degradation, and each notification is traceable end-to-end thanks to a unique identifier.

## Slide 24
I will now present a demonstration of the system in action. We will follow the complete journey of a message: I will publish a consumption event in RabbitMQ, observe its real-time processing in the Flink interface, find its trace in the Kibana logs, and finally visualize its impact on the Grafana metrics.

## Slide 25
Here I publish a *fairuse* message for a customer reaching 85% of their quota. We immediately observe, in the RabbitMQ management interface, the message being consumed. In the Flink UI, the job topology shows the data flowing through the various operators: source, validation, subscription retrieval, AI prediction, Erable call, and state update. The job is indeed in the *Running* state.

## Slide 26
In Kibana, I search for our message by its unique identifier. We find its full trace: the successful retrieval of the subscription, the prediction score, then the successful send to Erable. Each log is structured and enriched with fields such as customCode, MSISDN, or SID, allowing precise filtering. I can also, for example, isolate all Erable access errors via code 5030.

## Slide 27
Finally, on Grafana, we visualize in real time the impact of our message: the counter of sent notifications increments. The dashboards cover the job health and its quality of service, the event metrics, the cluster resource consumption, and the flow statistics per operator. In case of an anomaly, an alerting rule would immediately notify the team. This concludes the demonstration.

## Slide 28
In terms of results, the solution transforms a twice-daily batch process into real-time processing. It brings automatic scalability, fault tolerance guaranteed by checkpoints and high availability, predictive intelligence thanks to the AI module, and complete observability of the system.

## Slide 29
The prediction model achieves high performance: satisfactory precision and recall, an AUC-ROC above 0.92, for an inference latency below one millisecond — perfectly compatible with stream processing.

## Slide 30
To conclude, this project allowed me to design and implement a complete real-time data processing pipeline, combining Apache Flink and Kubernetes, and integrating an AI layer directly into the stream. It was a very enriching experience, combining the theory acquired during my studies with concrete hands-on practice on cutting-edge technologies.

## Slide 31
Several avenues for improvement are possible: adaptive retraining of the model using production data to improve its accuracy, the implementation of A/B testing between several model versions, the extension of scaling to other use cases, and the integration of new data sources to enrich the predictions.

## Slide 32
Thank you for your attention, and I remain at your disposal to answer any questions.

