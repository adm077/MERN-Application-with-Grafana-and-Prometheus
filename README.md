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

   - Prometheus Configuration:

   - To configure Prometheus to scrape metrics from Node Exporter and Jenkins, you need to modify the prometheus.yml file. Here is an example prometheus.yml configuration for your setup:
   ```
   global:
     scrape_interval: 15s

   scrape_configs:
     - job_name: 'node_exporter'
    static_configs:
      - targets: ['localhost:9100']

   ```
   - Check the validity of the configuration file:
   ```
   promtool check config /etc/prometheus/prometheus.yml
   ```
   - Reload the Prometheus configuration without restarting:
   ```
   curl -X POST http://localhost:9090/-/reload
   ```

   #Install Grafana on Ubuntu 22.04 and Set it up to Work with Prometheus

   Step 1: Install Dependencies:

   - First, ensure that all necessary dependencies are installed:
   ```
   sudo apt-get update
   sudo apt-get install -y apt-transport-https software-properties-common
   ```
   Step 2: Add the GPG Key:

   - Add the GPG key for Grafana:
   ```
   wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
   ```
   Step 3: Add Grafana Repository:

   - Add the repository for Grafana stable releases:
   ```
   echo "deb https://packages.grafana.com/oss/deb stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
   ```
   Step 4: Update and Install Grafana:

   - Update the package list and install Grafana:
   ```
   sudo apt-get update
   sudo apt-get -y install grafana
   ```
   Step 5: Enable and Start Grafana Service:

   - To automatically start Grafana after a reboot, enable the service:
   ```
   sudo systemctl enable grafana-server
   sudo systemctl start grafana-server
   sudo systemctl status grafana-server
   ```
   Step 6: Access Grafana Web Interface:

   - Open a web browser and navigate to Grafana using your server's IP address. The default port for Grafana is 3000. For example:
   ```
   http://<your-server-ip>:3000
   ```
   - You'll be prompted to log in to Grafana. The default username is "admin," and the default password is also "admin."

   Step 7: Change the Default Password:

   - When you log in for the first time, Grafana will prompt you to change the default password for security reasons. Follow the prompts to set a new password.

   Step 8: Add Prometheus Data Source:

   - To visualize metrics, you need to add a data source. Follow these steps:

   Click on the gear icon (⚙️) in the left sidebar to open the "Configuration" menu.

   Select "Data Sources."

   Click on the "Add data source" button.

   Choose "Prometheus" as the data source type.

   In the "HTTP" section:

   Set the "URL" to http://localhost:9090 (assuming Prometheus is running on the same server).
   
   ![image](https://github.com/adm077/MERN-Application-with-Grafana-and-Prometheus/assets/139608052/371ece96-f2c0-44d8-bedd-c623921767eb)

   Log Aggregation:

   Step 09: Import a Dashboard:

   - To make it easier to view metrics, you can import a pre-configured dashboard. Follow these steps:

   Click on the "+" (plus) icon in the left sidebar to open the "Create" menu.

   Select "Dashboard."

   Click on the "Import" dashboard option.

   Enter the dashboard code you want to import (e.g., code 1860).

   Click the "Load" button.

   Select the data source you added (Prometheus) from the dropdown.

   Click on the "Import" button.

   - You should now have a Grafana dashboard set up to visualize metrics from Prometheus.

   - Grafana is a powerful tool for creating visualizations and dashboards, and you can further customize it to suit your specific monitoring needs.

   - Integrate a log aggregation system (such as Loki, ELK Stack, or Fluentd) to collect and visualize logs from the MERN application.

   - Create a dashboard in Grafana to explore and analyze these logs.

   ![image](https://github.com/adm077/MERN-Application-with-Grafana-and-Prometheus/assets/139608052/f06ba0c8-4f7d-4839-9fd1-fac712a5f8b0)
   ![image](https://github.com/adm077/MERN-Application-with-Grafana-and-Prometheus/assets/139608052/77d1997e-98ca-403f-9b51-2893dd4f27fc)
   ![image](https://github.com/adm077/MERN-Application-with-Grafana-and-Prometheus/assets/139608052/721a9072-b216-4e69-bffc-c62babda0c94)




   Implement Distributed Tracing:

   - Set up distributed tracing in the application using tools like Jaeger or Zipkin.

   - Integrate tracing data into Grafana for a full view of request flows through the application stack.

   Alerting and Anomaly Detection:

   - Develop sophisticated alerting rules in Grafana based on application-specific metrics and log patterns.

   - Explore anomaly detection with Grafana, using the gathered metrics and logs.

   Documentation and Analysis:

   - Document the entire setup process, including challenges and how they were addressed.

   - Provide an analysis of the application performance and behavior based on the collected metrics, logs, and traces.

   - Include screenshots and explanations of the Grafana dashboards.

   Bonus (Optional):

   - Implement auto-scaling mechanisms based on specific metrics.

   - Explore service mesh integration (like Istio or Linkerd) for more advanced observability features.

Deliverables:

- A detailed report documenting the monitoring setup, with a focus on how it integrates with each part of the MERN stack.
- Screenshots and explanations of the Grafana dashboards, highlighting key metrics, logs, and trace visualizations.
- An analysis of the application's performance and behavior based on the observed data.
