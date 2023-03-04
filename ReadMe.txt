--Steps for Grafana installation--

1. sudo apt-get install -y apt-transport-https curl

2. curl https://packages.grafana.com/gpg.key | sudo apt-key add -

3. echo "deb https://packages.grafana.com/oss/deb stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list

4. sudo apt-get update

5. sudo apt-get install -y grafana

6. sudo systemctl start grafana-server

7. sudo systemctl status grafana-server

8. sudo systemctl enable grafana-server
