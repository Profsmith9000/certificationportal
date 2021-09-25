# Node Monitoring with Prometheus

```bash
mainnet-config.json
```
```bash
nano mainnet-config.json
```
```bash
nano mainnet-config.json
```
## In this file search for
"hasPrometheus": [
"127.0.0.1",
12798
],
________________________________________________________________________________________________
# Replace the IP address 127.0.0.1 with 0.0.0.0 to allow listening for external connections.
I should look like this afterwards... 
"hasPrometheus": [
"0.0.0.0,
12798
],

```bash
cd ~
mkdir Downloads
cd Downloads
```
```bash
wget https://github.com/prometheus/node_exporter/releases/download/v1.0.1/node_exporter-1.0.1.linux-amd64.tar.gz
tar xvfz node_exporter-1.0.1.linux-amd64.tar.gz
rm node_exporter-1.0.1.linux-amd64.tar.gz
cd node_exporter-1.0.1.linux-amd64
```
```bash
sudo ufw allow proto tcp from IP.OF.MONITORING.SERVER to any port 9100
sudo ufw allow proto tcp from IP.OF.MONITORING.SERVER to any port 12798
```
```bash
./node_exporter
```
# You are now preparing your monitoring server
```bash
cd ~
mkdir Downloads
cd Downloads
wget https://github.com/prometheus/prometheus/releases/download/v2.22.2/prometheus-2.22.2.linux-amd64.tar.gz
tar xvfz prometheus-2.22.2.linux-amd64.tar.gz
rm prometheus-2.22.2.linux-amd64.tar.gz
cd prometheus-2.22.2.linux-amd64/
```
```bash
nano prometheus.yml

```
# We stick as closely as possible to official cardano documentation. Edit your file to look like this:
________________________________________________________________________________________________
# my global config
```bash
global:
scrape_interval: 15s # Set the scrape interval to every 15 seconds. Default is every 1 minute.
evaluation_interval: 15s # Evaluate rules every 15 seconds. The default is every 1 minute.
external_labels:
monitor: 'codelab-monitor'
```
# scrape_timeout is set to the global default (10s).
________________________________________________________________________________________________
# Alertmanager configuration
```bash
alerting:
alertmanagers:
- static_configs:
- targets:
```
# - alertmanager:9093
________________________________________________________________________________________________
# Load rules once and periodically evaluate them according to the global 'evaluation_interval'.
rule_files:
# - "first_rules.yml"
# - "second_rules.yml"
________________________________________________________________________________________________
# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
# The job name is added as a label `job=` to any timeseries scraped from this config.
```bash

```
```bash

```
```bash

```
```bash

```
```bash

```





















































