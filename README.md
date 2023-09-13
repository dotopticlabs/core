# Welcome to the DECO Stack
The DOCHE/DOCSE Stack is a powerful open-source observability platform designed to revolutionize the way you manage and configure your observability data. Comprising four essential components - DotOptic, OpenTelemetry, Cassandra, Hadoop/S3, and ElasticSearch - this stack empowers users to seamlessly orchestrate and centralize their observability configurations.

Metrics – “Is there a problem?”

Logs – “What is the problem?”

Traces – “Where is the problem?”

## About DotOptic
At the heart of the DOCHE/DOCSE Stack is DotOptic, an open-source API server meticulously crafted for configuring observability data with unparalleled ease and efficiency. DotOptic specializes in "Configuration as Code," allowing users to define their observability settings in a single, version-controlled YAML file stored in a GitHub repository.

## Configuration as Code
With DotOptic, you can specify a unique configuration ID, much like a YAML file in a GitHub repository. This ID acts as a focal point for all your observability configurations, ensuring that your settings are consistent, organized, and readily accessible. DotOptic automatically detects changes in your GitHub repository, ensuring that your observability configurations remain up-to-date without manual intervention.

## Seamless Synchronization
One of the standout features of the DECH Stack is its ability to maintain perfect synchronization between your GitHub repository and DotOptic's configuration dashboard. When you update your configuration settings from the dashboard, DotOptic seamlessly pushes these changes to GitHub, ensuring that your "Observability as Code" is always current and accurate.

## Core Services
While DotOptic plays a central role in the DECH Stack, it collaborates seamlessly with Elasticsearch, Cassandra, Hadoop Distributed File System (HDFS), and PostgreSQL to deliver a robust and comprehensive observability solution. These services work in unison to provide a seamless and efficient platform for managing your observability data.

Experience the future of observability management with the DECH Stack, where Configuration as Code meets automation, and observability becomes a breeze. Dive into our documentation to get started and unleash the full potential of your observability journey.

# Observability Configuration as Code

At the core of DotOptic is the notion of observability configuration as code, meaning that all configuration about the collection, retention, and analysis of all metrics, traces, and logs of a particular service or application are configured in a Github repository that is version controlled and always kept in sync with the version that DotOptic is using for data processing. This also means when a configuration is updated in the DotOptic dashboard, DotOptic will push a change to the repo holding that configuration file as a commit from the user "DotOptic Dashboard - Username".

When installing DotOptic, DotOptic will collect and ingest as much data as possible to be later filtered out for retention based on configuration. There are 19 main types of configurations to be had in the application.

1. Logging Configuration:
2. Monitoring Configuration:
3. Tracing Configuration:
4. Instrumentation:
5. Data Storage:
6. Security and Access Control:
7. Visualization and Dashboards:
8. Alerting and Notifications:
9. Resource Allocation:
10. Error Handling and Recovery:
11. Data Sampling and Aggregation:
12. Integration with CI/CD Pipeline:
13. Compliance and Auditing:
14. Documentation and Metadata:
15. Cost Management:
16. Backup and Disaster Recovery:
17. Performance Tuning:
18. Data Enrichment:
19. Localization and Internationalization:

## Logging Configuration:

An example of logging configuration may look like the following:

```
service-name: my-service
logging:
  # Log levels: debug, info, error
  log_level:
    - debug
    - info
    - error

  # Log format and structure (multiline)
  log_format: >
    '{ "timestamp": "%timestamp%",
      "level": "%level%",
      "message": "%message%" }'

  # Log retention policies for different destinations
  retention_policy:
    # Retention policy for Elasticsearch
    - type: time
      destination: elasticsearch
      duration: 90 days

    # Retention policy for Cassandra
    - type: time
      destination: cassandra
      duration: 90 days

    # Retention policy for HDFS
    - type: time
      destination: hdfs
      duration: 2 years

    # Retention policy for AWS Glacier
    - type: time
      destination: aws_glacier
      duration: 4 years

  # Log destination (HDFS cluster)
  log_destination:

    - name: hdfs
      type: hdfs
      host: hdfs-cluster.example.com
      port: 8020
      path: /logs/myapp

    # Log destination (Cassandra)
    - name: cassandra
      type: cassandra
      host: cassandra-cluster.example.com
      keyspace: myapp_logs
      table: logs

    # Log destination (Elasticsearch)
    - name: elasticsearch
      type: elasticsearch
      host: elasticsearch.example.com
      port: 9200
      index_prefix: myapp-logs-

    # Log destination (AWS Glacier)
    - type: aws_glacier
      aws_region: us-east-1
      vault_name: myapp-logs-vault
```

## Monitoring Configuration

An example of monitoring metrics and the configuration of such is the following:

```
service-name: my-service
monitoring:
  # Metric types
  metric_types:
    - name: CPU
      enabled: true
      interval: 60 seconds
      alerting_threshold: 90%
      alert_channel: slack
      anomaly_detection:
        - algorithm: z-score
          threshold: 2.0
          window_size: 15 minutes
          alert_channel: pagerduty

    - name: Memory
      enabled: true
      interval: 60 seconds
      alerting_threshold: 95%
      alert_channel: email
      anomaly_detection:
        - algorithm: moving_median_absolute_deviation
          threshold: 3.0
          window_size: 30 minutes
          alert_channel: slack

    - name: Disk Space
      enabled: true
      interval: 300 seconds
      alerting_threshold: 85%
      alert_channel: pagerduty
      anomaly_detection:
        - algorithm: exponential_moving_average
          threshold: 5%
          window_size: 1 hour
          alert_channel: slack

    - name: Network Throughput
      enabled: true
      interval: 300 seconds
      alerting_threshold: 90%
      alert_channel: slack
      anomaly_detection:
        - algorithm: z-score
          threshold: 2.5
          window_size: 30 minutes
          alert_channel: email

    - name: Server Uptime
      enabled: true
      interval: 60 seconds
      alerting_threshold: 99.9%
      alert_channel: email
      anomaly_detection:
        - algorithm: z-score
          threshold: 2.0
          window_size: 15 minutes
          alert_channel: slack

    - name: Load Average
      enabled: true
      interval: 60 seconds
      alerting_threshold: 2.0
      alert_channel: slack
      anomaly_detection:
        - algorithm: moving_median_absolute_deviation
          threshold: 2.0
          window_size: 15 minutes
          alert_channel: pagerduty

    - name: Request Count
      enabled: true
      interval: 300 seconds
      alerting_threshold: 1000 requests/minute
      alert_channel: email
      anomaly_detection:
        - algorithm: exponential_moving_average
          threshold: 800 requests/minute
          window_size: 30 minutes
          alert_channel: slack

    - name: Response Time
      enabled: true
      interval: 300 seconds
      alerting_threshold: 500 milliseconds
      alert_channel: pagerduty
      anomaly_detection:
        - algorithm: z-score
          threshold: 2.0
          window_size: 15 minutes
          alert_channel: slack

    - name: Error Rate
      enabled: true
      interval: 300 seconds
      alerting_threshold: 5%
      alert_channel: slack
      anomaly_detection:
        - algorithm: exponential_moving_average
          threshold: 10%
          window_size: 30 minutes
          alert_channel: email

    - name: User Sign-Ups
      enabled: true
      interval: 300 seconds
      alerting_threshold: 50 per hour
      alert_channel: email
      anomaly_detection:
        - algorithm: z-score
          threshold: 2.0
          window_size: 1 hour
          alert_channel: slack

    - name: Database Queries
      enabled: true
      interval: 300 seconds
      alerting_threshold: 1000 queries/minute
      alert_channel: slack
      anomaly_detection:
        - algorithm: moving_median_absolute_deviation
          threshold: 3.0
          window_size: 45 minutes
          alert_channel: pagerduty

# Alerting channels
alerting_channels:
  - name: email
    type: email
    recipients:
      - admin@example.com
      - ops@example.com

  - name: slack
    type: slack
    channel: #general
    api_key: your-slack-api-key-here

  - name: pagerduty
    type: pagerduty
    service_key: your-pagerduty-service-key-here
```

Here's an explanation of the three anomaly detection algorithms you mentioned: z-score, moving median absolute deviation, and exponential moving average:

1.  **Z-Score Anomaly Detection:**
    
    -   **Algorithm Explanation:** The z-score is a statistical measure that quantifies how far a data point is from the mean of a dataset in terms of standard deviations. In anomaly detection, the z-score is used to identify data points that significantly deviate from the mean.
        
    -   **Anomaly Detection Process:**
        
        -   Calculate the mean (average) and standard deviation of a time series dataset.
        -   For each data point in the time series, calculate its z-score using the formula: `Z = (X - μ) / σ`, where `X` is the data point, `μ` is the mean, and `σ` is the standard deviation.
        -   Data points with a high absolute z-score (typically greater than a predefined threshold) are considered anomalies.
    -   **Use Case:** Z-score anomaly detection is suitable for identifying anomalies in normally distributed data, such as system metrics like CPU utilization or response times. It's effective at detecting data points that are unusually far from the mean.
        
2.  **Moving Median Absolute Deviation (MAD) Anomaly Detection:**
    
    -   **Algorithm Explanation:** The moving median absolute deviation (MAD) is a robust statistical method for detecting anomalies in time series data. It measures the variability of data points around the median and identifies outliers.
        
    -   **Anomaly Detection Process:**
        
        -   Calculate the median of a sliding window of recent data points.
        -   For each data point, calculate its absolute deviation from the median.
        -   Calculate the median of these absolute deviations over the sliding window.
        -   Data points with absolute deviations significantly greater than the median absolute deviation are considered anomalies.
    -   **Use Case:** MAD is suitable for detecting anomalies in data with non-normal distributions or data that may have outliers. It's robust because it's less sensitive to extreme values compared to mean-based methods.
        
3.  **Exponential Moving Average (EMA) Anomaly Detection:**
    
    -   **Algorithm Explanation:** The exponential moving average (EMA) is a smoothing technique that calculates a weighted average of recent data points. In anomaly detection, it's used to identify deviations from the expected trend in time series data.
        
    -   **Anomaly Detection Process:**
        
        -   Calculate an exponentially weighted moving average of the time series data, giving more weight to recent data points.
        -   Calculate the difference between each data point and its corresponding EMA value.
        -   Data points with differences exceeding a predefined threshold are considered anomalies.
    -   **Use Case:** EMA-based anomaly detection is useful for identifying deviations from expected trends in data. It can be applied to various metrics, including application performance, network traffic, and financial data.
  
## Tracing Configurations:

An example of tracing configuration for a service is the following:

```
service_name: my-service
instrumentation:
  - name: otel_tracing
    type: trace
    settings:
      enabled: true
      # Instrumentation options (if applicable)

exporters:
  dotoptic_custom_backend:
    type: dotoptic_custom_backend  # Specify the exporter type for "dotoptic"
    settings:
      # Configure settings specific to your "dotoptic" exporter
      api_key: your-api-key
      endpoint: http://your-dotoptic-backend-endpoint
      # Other settings specific to "dotoptic" exporter

processors:
  batch:
    send_batch_size: 512  # Specify the maximum number of spans to include in a batch
    timeout: 5s  # Specify the maximum time to wait for a batch to be sent

resource:
  attributes:
    service.name: my-service
    service.version: 1.0.0
    host.name: my-host
    host.ip: 192.168.1.100
    environment: production
    cloud.provider: AWS
    cloud.region: us-east-1
    runtime.name: Node.js
    runtime.version: 14.17.3
    custom_key: custom_value

sampler:
  name: probability
  probability: 0.10  # 10% sampling rate
```

Also, possible sampler configuration could varry by the following:
1. *Always On:* This strategy samples all traces, meaning every trace is collected and exported. It's useful for debugging and ensuring that you capture every trace but can generate a large volume of data.
2. *Always Off:* This strategy never samples any traces. It's useful when you don't want to collect trace data but still have OpenTelemetry instrumentation enabled.
3. *Probability-based Sampling:* In this approach, you configure a fixed probability for sampling (e.g., 1% of traces). It randomly selects traces to sample based on this probability. This helps reduce the volume of trace data while providing statistical insights.
4. *Rate-based Sampling:* Similar to probability-based sampling, but you specify a fixed rate at which traces are sampled (e.g., 1 trace per second). This helps control the rate of data generation while maintaining a consistent flow of trace data.
5. *Parent-based Sampling:* This strategy examines the attributes or context of incoming requests and decides whether to sample based on certain conditions. For example, you might sample only traces that originate from specific services or have specific attributes.
6. *Dynamic Sampling:* Dynamic sampling strategies adjust the sampling rate based on the current load or other criteria. This can help ensure that you collect more traces during periods of high traffic and fewer traces during low traffic.

# Archetechture

## Overview
![DotOptic Archetechture](https://i.imgur.com/z0c1NIs.png "Image Description")

The architecture of this application is defined by a number of data-stores (Cassandra, HDFS, ElasticSearch, PostgreSQL, Redis), third-party services (AWS Glacier, Github), and stateless, horizontally scalable services that will be deployed onto Docker Swarm. The services and their general purposes are outlined bellow:

It should be noted that the main data-stores for services are Cassandra, HDFS, and PostgreSQL. For all intents and purposes, Observabiity Configuration Managment should have one YAML file (or section of an entire application config) per Service or Server that a client wants to collect observability on. This should also respectfully correspond with one row in a Postgres RDMS for that client of which a client may have many services or servers. This should also correspond to one Github repository for the configuration (unless the client is choosing to keep all of the configurations in one repo). In terms of scalability for the scale that DotOptic plans to become, this utalization of Postgres for this amount of relational data should be sufficient and feasible for a single RDMS. It should also be noted, however, that this is for configuration alone (not log data) as log data will be stored, instead, primarily on Cassandra (where it can be rotated by the TimeWindowCompactionStrategy method for data retention and further stored on HDFS for the foreseeable future. Note that HDFS is considered comparable for log storage as a service like AWS S3 and may be replaced for that service in the future. Also note that while it is traditionally feasible to store log data in ElasticSearch, storing log data from possibly 1000s of client applications in one single ElasticSearch instance is not feasible, and thus the use of HDFS or AWS S3 is used. ElasticSearch is, however, used for storing the log data of the most recent logs per client (before they are rotated) so clients can have a searchable index of logs (including metrics, traces, and other information). This layout, however, requires a client to have their own ElasticSearch instance which will be provisioned likely one per client or one per service which will be rotated on a regular configured basis.

In addition to the data-stores and distributed services, there is a front-faceing ELB (Elastic Load Balancer) and an Edge Router responsible for load-balancing requests and routing them based on sub-domain per service. The API Service will be hosted on something like “api.dotoptic.com”, whereas the Log Ingestion Service will be hosted on something like “logs.dotoptic.com”, etc. Multiple lines and arrows may have the same number label associated with them, but equate to the same request, especially if they are going through the ELB and Edge Router to their final service destination.

Services with a dashed-border refer to services that are exposed on the ELB. These services are the Web-Server Service, the API Service, and the Log Ingestion Service. All other services are internal and not externally facing.

*The following is a list of all the services in the application and their description.*

**Log Rotation and Cleanup Service:** This service is responsible for reading Observability Configuration (stored in PostgreSQL), and rotating logs from Cassandra to HDFS. It is of the intent to rotate logs in this manner when logs are no longer needed for either live updates or dashboard views, meaning that if a dashboard view only intends to go back 100 days for something like, for example, a graph, then logs are rotated to HDFS once they are older than 100 days. Once on HDFS, they will remain long term and possibly indefinitely unless beyond a prohibitive time period (such as 5 years) in which case a cost analysis will be made on the likelihood of ever having a bulk reporting request query the data and the cost of retrieving it back from AWS Glacier (used typically for disasster recovery). Moving from Cassandra to HDFS or S3 can be thought of as the “live queryable archive”.

**Disaster Recover Rotation Service:** The disaster recovery service is the rotation mechanism for logs that have been stored in HDFS for AWS S3 and are likely not to be queried by even a long-term reporting request (i.e., scripts older than 1 year old if the maximum date a report needs to be generated reaches back a maximum of 1 year). Once it is no longer likely that any kind of read from this will take place, it is moved to AWS Glacier (which is incredibly cheap to write to but incredibly expensive to read back from, because it is literally written on tape). Although this does not mean the data is archived forever, it does mean that it’s usually not ever intended to be read ever again. Moving from HDFS or S3 to AWS Glacier can be thought of as the “reporting queryable archive”.

**ElasticSearch Rotation Service:** Clients will require a text-searchable index for log data, especially recent logs for likely a period of 1-3 months. Because of this, log data (which also includes data for things like traces, metrics, etc) will be replicated from Cassandra (where it is usually stored to perform queries such as generating graphs, recent reports, analytics, etc based on the efficient reading off disk of a keyspace withing the architecture of the Bloom Filter that is Cassandra). When data is rotated out of ElasticSearch, it is simply deleted and purged, leaving an ElasticSearch database with a limited size, depending on whatever is configured (which is read from PostgreSQL).
Log Ingestion Service: The Log Ingestion Service is simply the endpoint or service which log data from various sources such as client containers, servers, databases, code, and other infrastructure, point their data to be ingested and written to Cassandra. Notice, however, that this service cannot be limited or even read PostgreSQL configuration as doing so would imply that every log request would require a read of PostgreSQL. Because of this, the Log Ingestion Service must simply write all logs to Cassandra, no matter their source, permissions, or where they came from. If these logs happen to be unconnected or not associated with any account, or are a result of legacy configuration or misconfiguration, this service must still ingest the logs and write them to Cassandra. It is for this reason that the Log Rotation and Cleanup is called “Cleanup”, because it’s primary job is to remove log data that is not associated with any account or may be a leftover from legacy code from a now unused account.

**API Server Service:** The API Server service is simply the stateless, API endpoint for all API requests. This service will utalize Redis for session storage as Redis is a highly scalable key value store meant for a purpose just as this. This also prevents the API server from making continuous repetitive requests to the RDMS who’s traffic must be sparingly used. The API server is responsible for sending configuration on various clients log ingestion, fetching configuration from Github and syncing it with the PostgreSQL RDMS, pushing to Github, generating API responses for the web server for all user dashboard requests such as reports, graphs, alerts, and notifications, and possibly even analysis and report generation unless those services are ported to another service later on.

**Web Server Service:** The web-server service is simply the web-server responsible for rendering the user-dashboard and front-end graphical user interface. It is stateless and horizontally scalable and relies entirely on requests made to the API server.
