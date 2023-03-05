--Steps for Grafana installation--

1. sudo apt-get install -y apt-transport-https curl

2. curl https://packages.grafana.com/gpg.key | sudo apt-key add -

3. echo "deb https://packages.grafana.com/oss/deb stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list

4. sudo apt-get update

5. sudo apt-get install -y grafana

6. sudo systemctl start grafana-server

7. sudo systemctl status grafana-server

8. sudo systemctl enable grafana-server


--Steps for Prometheus installation--

1. wget https://github.com/prometheus/prometheus/releases/download/v2.42.0/prometheus-2.42.0.linux-amd64.tar.gz

2. tar xvf prometheus-2.42.0.linux-amd64.tar.gz

3. sudo mv prometheus-2.42.0.linux-amd64 /usr/local/bin/prometheus

4. Create a Prometheus configuration file /etc/prometheus/prometheus.yml and add the following contents:

global:
  scrape_interval:     15s
  evaluation_interval: 15s

scrape_configs:
  - job_name: 'prometheus'
    scrape_interval: 5s
    static_configs:
      - targets: ['localhost:9090']

5. Create a Prometheus service file /etc/systemd/system/prometheus.service and add the following contents:

[Unit]
Description=Prometheus Server
Documentation=https://prometheus.io/docs/introduction/overview/
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/local/bin/prometheus/prometheus --config.file /etc/prometheus/prometheus.yml --storage.tsdb.path /var/lib/prometheus/
ExecReload=/bin/kill -HUP $MAINPID
Restart=always

[Install]
WantedBy=multi-user.target

6. mkdir /var/lib/prometheus

7. sudo useradd -rs /bin/false prometheus

8. sudo chown -R prometheus:prometheus /usr/local/bin/prometheus

9. sudo chown -R prometheus:prometheus /etc/prometheus

10. sudo chown -R prometheus:prometheus /var/lib/prometheus

11. sudo systemctl daemon-reload

12. sudo systemctl start prometheus

13. sudo systemctl enable prometheus



==Install Node Exporter==

1. wget https://github.com/prometheus/node_exporter/releases/download/v1.5.0/node_exporter-1.5.0.linux-amd64.tar.gz

2. tar xvf node_exporter-1.5.0.linux-amd64.tar.gz

3. mv node_exporter-1.5.0.linux-amd64 /usr/local/bin/

4. sudo useradd --no-create-home --shell /bin/false node_exporter

5. vi /etc/systemd/system/node_exporter.service

[Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target

[Service]
User=node_exporter
Group=node_exporter
Type=simple
ExecStart=/usr/local/bin/node_exporter-1.5.0.linux-amd64/node_exporter

[Install]
WantedBy=multi-user.target


6. sudo systemctl daemon-reload

7. sudo systemctl enable node_exporter

8. systemctl start node_exporter

9. Log into Prometheus installed server and vi /etc/prometheus/prometheus.yml file and add the node exporter installed server's ip and port under target section and restart prometheus service.
