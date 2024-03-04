# MERN-Application-with-Grafana-and-Prometheus

Objective:

Develop an advanced monitoring solution for a MERN application using Grafana and Prometheus. This assignment includes setting up detailed monitoring for a full-stack JavaScript application, including database, backend, and frontend performance metrics, as well as log aggregation and tracing.


Assignment Tasks:

1. MERN Application Setup:

   - Deployed a travel memory application in the local machine.
   - Cloned the reposetory & configured backend & frontend with all the dependency.

![image](https://github.com/adm077/MERN-Application-with-Grafana-and-Prometheus/assets/139608052/68481749-c78f-4c46-8143-36f46d66b431)
![image](https://github.com/adm077/MERN-Application-with-Grafana-and-Prometheus/assets/139608052/8a4563df-df49-444e-a7c4-e18a5dab120e)
![image](https://github.com/adm077/MERN-Application-with-Grafana-and-Prometheus/assets/139608052/c9c0f0ee-fbbc-4b12-9859-3800ec83e4aa)


2. Integrate Prometheus:

   - Integrate Prometheus metrics into the Node.js backend. Use client libraries to expose custom metrics like API response times, request counts, and error rates.
   - First, create a dedicated Linux user for Prometheus and download Prometheus on the prometheus node
   ```
   sudo useradd --system --no-create-home --shell /bin/false prometheus
   wget https://github.com/prometheus/prometheus/releases/download/v2.47.1/prometheus-2.47.1.linux-amd64.tar.gz
   ```
   - Extract Prometheus files, move them, and create directories:
   ```
   tar -xvf prometheus-2.47.1.linux-amd64.tar.gz
   cd prometheus-2.47.1.linux-amd64/
   sudo mkdir -p /data /etc/prometheus
   sudo mv prometheus promtool /usr/local/bin/
   sudo mv consoles/ console_libraries/ /etc/prometheus/
   sudo mv prometheus.yml /etc/prometheus/prometheus.yml
   ```
   - Set up MongoDB monitoring using an exporter, such as MongoDB Exporter, to track database performance.
   - Set ownership for directories:
   ```
   sudo chown -R prometheus:prometheus /etc/prometheus/ /data/
   ```
   - Create a systemd unit configuration file for Prometheus:
   ```
   sudo nano /etc/systemd/system/prometheus.service
   ```
   - Add the following content to the prometheus.service file:
   ```
   [Unit]
   Description=Prometheus
   Wants=network-online.target
   After=network-online.target

   StartLimitIntervalSec=500
   StartLimitBurst=5

   [Service]
   User=prometheus
   Group=prometheus
   Type=simple
   Restart=on-failure
   RestartSec=5s
   ExecStart=/usr/local/bin/prometheus \
     --config.file=/etc/prometheus/prometheus.yml \
     --storage.tsdb.path=/data \
     --web.console.templates=/etc/prometheus/consoles \
     --web.console.libraries=/etc/prometheus/console_libraries \
     --web.listen-address=0.0.0.0:9090 \
     --web.enable-lifecycle

   [Install]
   WantedBy=multi-user.target
   ```
   - Enable and start Prometheus:
   ```
   sudo systemctl enable prometheus
   sudo systemctl start prometheus
   ```
2. Enhance Grafana Dashboards:

   - Create advanced Grafana dashboards to display both the default and custom metrics from the MERN application.

   - Include detailed visualizations for backend performance, database health, and frontend performance (if possible).
   
   ![image](https://github.com/adm077/MERN-Application-with-Grafana-and-Prometheus/assets/139608052/1dcba896-4850-420a-8bea-d7b254e71416)


3. Log Aggregation:

   - Integrate a log aggregation system (such as Loki, ELK Stack, or Fluentd) to collect and visualize logs from the MERN application.

   - Create a dashboard in Grafana to explore and analyze these logs.

4. Implement Distributed Tracing:

   - Set up distributed tracing in the application using tools like Jaeger or Zipkin.

   - Integrate tracing data into Grafana for a full view of request flows through the application stack.

5. Alerting and Anomaly Detection:

   - Develop sophisticated alerting rules in Grafana based on application-specific metrics and log patterns.

   - Explore anomaly detection with Grafana, using the gathered metrics and logs.

6. Documentation and Analysis:

   - Document the entire setup process, including challenges and how they were addressed.

   - Provide an analysis of the application performance and behavior based on the collected metrics,            logs, and traces.

   - Include screenshots and explanations of the Grafana dashboards.

7. Bonus (Optional):

   - Implement auto-scaling mechanisms based on specific metrics.

   - Explore service mesh integration (like Istio or Linkerd) for more advanced observability features.

Deliverables:

- A detailed report documenting the monitoring setup, with a focus on how it integrates with each part of the MERN stack.
- Screenshots and explanations of the Grafana dashboards, highlighting key metrics, logs, and trace visualizations.
- An analysis of the application's performance and behavior based on the observed data.
