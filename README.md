# Node Exporter Setup for Metrics Collection in Prometheus

This document describes how to install and configure the Node Exporter on a server so that metrics can be collected by Prometheus.

## Step 1: Install Node Exporter

1. **Download the latest version of Node Exporter:**
    
    ```bash
    wget <https://github.com/prometheus/node_exporter/releases/download/v1.8.2/node_exporter-1.8.2.linux-amd64.tar.gz>
    
    ```
    
    (can be found here: Node Exporter Download)
    
2. Extract the downloaded file:
    
    ```bash
    tar xvfz node_exporter-1.8.2.linux-amd64.tar.gz
    ```
    

1. Move it to a common directory:

    
    ```bash
    mv node_exporter-1.8.2.linux-amd64/node_exporter /usr/local/bin/
    ```
    

## Step 2: Configure Node Exporter as a Service

1. Create a service file for Node Exporter:
    
    ```bash
    nano /etc/systemd/system/node_exporter.service
    ```
    

2. Add the configurations to the file:

   ```yaml
   [Unit]
   Description=Node Exporter
   Wants=network-online.target
   After=network-online.target

   [Service]
   User=node_exporter # Check the desired user
   ExecStart=/usr/local/bin/node_exporter

   [Install]
   WantedBy=default.target
   ```

1. Create the user and adjust permissions:
    
    ```bash
    useradd -rs /bin/false node_exporter
    ```
    

1. Start the service, enable it to start automatically, and check the status:
    
    ```bash
    sudo systemctl daemon-reload
    sudo systemctl start node_exporter
    sudo systemctl enable node_exporter
    sudo systemctl status node_exporter
    ```
    

## Step 3: Add the configuration on the Prometheus Server

1. In the `prometheus.yml` file, add your job configuration in the `scrape_configs` section:
    
    ```yaml
    - job_name: "node_exporter"
      static_configs:
        - targets: ["SERVER_IP:9100"]
    ```
    

1. Restart the service:
    
    ```bash
    sudo systemctl restart prometheus
    ```
    

This `README.md` file provides all the necessary information to set up the Node Exporter on a server and integrate it with Prometheus.
