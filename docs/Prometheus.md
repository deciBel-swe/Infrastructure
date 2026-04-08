Source:
    - TechWorld with Nana: https://www.youtube.com/watch?v=h4Sl21AKiDg

---
What is Prometheus:
    Used in Container and microservices monitoring but can be used for bare server applications also

Why Use Prometheus:
    Complexity: 
        - Alot of Processes which are interconnected (and distributed systems)
        - What is the problem? (HW or SW problem?, Resources, Errors, Latency, etc)
    
    Example of manual Debugging:
        - Container doesnt have enough memory was killed
        - DB was dependent on that container so now it crashed
        - Backend couldnt communicate with DB
        - Frontend has Error "Can't Login"

        Normal Manual debugging would be to look at front then back then DB then after all that you find the root of the problem
        but what is more productive is taht a service monitor all the services and alerts when something crashes, etc
        or even identify the problems before they occur

---
Prometheus architecture:

Prometheus Server:
    - Time Series Database:
        - Stores metrics data (CPU usage, num of Exceptions, etc)
    - Data Retrieval worker:
        - Pulls metrics data from apps, services and pushes them into the DB
    - HTTP server
        - Accepts PromQL queries to the data and display the output to any UI (Prometheus Web UI, Grafana, etc)

Targets and metrics:
    - Targets: What Prometheus monitors (linux servers, Apps, DBs, etc)
    - Metrics: 
        - Units that are monitored in those targets (CPU, Memory, Exceptions count, etc)
        - Human-readable text based
        - Metrics entries has 2 attributes
            - HELP: description of what the metric is 
            - TYPE: 3 metrics types
                - Counter: How many times "x" happened
                - Gauge: What is the current value of "x" now
                - Histogram: How long or how big
---
Collecting Metrics data from Targets:
    - Pulls from HTTP endpoints:
        - hostaddress/metrics
        - must be in correct format

Target Endpoints and Exporters:
    - Some services expose /metrics endpoints by default
    - otherwise needs an extra component (Exporter)
    - Exporter: https://prometheus.io/docs/instrumenting/exporters/
        - 1. Fetches metrics from target
        - 2. converts to correct format
        - 3. expose /metrics
        - 4. Prometheus scraps the data from the exporter

Monitoring your own applications
    - Client Libraries for different languages: https://prometheus.io/docs/instrumenting/clientlibs/
---
Pull mechanism
    Advantages: 
        - Unlike other montitoring services, its pull which is better (decreases network load etc)
        - Multiple Prometheus instances can pull metrics data
        - better detection/insight if service is up and running unlike in push where you don't know if its not running or network problem, etc
    Pushgateway:
        - When targets only for a short time to be scraped (batch jobs, scheduled jobs, etc)
        - For such jobs Prometheus offers push metrics
---
Configuring Prometheus:
    in prometheus.yml:
        - Global config
        - rule_files: aggregating metric values or creating alerts when condition met
        - scrape_configs: 
            - configures targets (can even monitor itself)
            - Define your own jobs
---
Alert Manager:
    - How does Prometheus trigger the alerts that was defined in rules and who recieves them
    - Component "AlertManager" Notifies by Email, Slack, etc
---
Prometheus Data Storage:
    - Where doeds Prometheus store the data
    - Disk (Local, Remote Storage System)
---
PromQL
    - Query target directly
    - use visualization tools (Grafana, etc)
