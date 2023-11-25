<p align="center">
  <a href="http://github.com/lzocateli">
    <img alt="GitHub followers" src="https://img.shields.io/github/followers/lzocateli?style=social">
  </a>
  <a href="http://twitter.com/lzocateli00">
    <img alt="Twitter Follow" src="https://img.shields.io/twitter/follow/lzocateli00?style=social">
  </a>
</p>


# nuuvify-monitor
Full stack tools for monitoring containers and other stuff.
- Netdata
- Prometheus
- AlertManager
- Docker
- cAdvisor
- Grafana
- Node_Exporter


# Install solution

First of all, clone the nuuvify-monitor repo:
```bash
# git clone https://github.com/lzocateli/nuuvify-monitor.git
```

## Install Docker and create Swarm cluster
```bash
# curl -fsSL https://get.docker.com | sh
# docker swarm init

```

## Install Netdata:
```bash
# bash <(curl -Ss https://my-netdata.io/kickstart.sh)
```

Setting Netdata Exporter configuration in Prometheus:
```bash
# nano config/prometheus/prometheus.yml
...
- job_name: 'netdata'
    metrics_path: '/api/v1/allmetrics'
    params:
      format: [prometheus]
    honor_labels: true
    scrape_interval: 5s
    static_configs:
         - targets: ['YOUR_IP:19999']
```


## Get Rocket.Chat Incoming WebHook 

1) Deploy nuuvify stack, only to get the WebHook

```bash
# docker stack deploy -c docker-compose.yml nuuvify
```

2) Access YOUR_IP:3080 and create your account

3) Login with your user and go to: Administration => Integrations => New Integration => Incoming WebHook

4) Set "Enabled" and "Script Enabled" to "True"

5) Set all channels, icons, etc. as you need



## Deploy Stack with Docker Swarm

Execute deploy to create the stack of nuuvify-monitor:
```bash
# docker stack deploy -c docker-compose.yml nuuvify

Creating network nuuvify_backend
Creating network nuuvify_frontend
Creating network nuuvify_default
Creating service nuuvify_prometheus
Creating service nuuvify_node-exporter
Creating service nuuvify_alertmanager
Creating service nuuvify_cadvisor
Creating service nuuvify_grafana
```

Verify if services are ok:
```bash
# docker service ls

ID              NAME                          MODE         REPLICAS  IMAGE                                  PORTS
2j5vievon95j    nuuvify_alertmanager         replicated   1/1       lzocateli/alertmanager_alpine:latest   *:9093->9093/tcp
y1kinszpqzpg    nuuvify_cadvisor             global       1/1       google/cadvisor:latest                 *:8080->8080/tcp
jol20u8pahlp    nuuvify_grafana              replicated   1/1       grafana/grafana:latest                 *:3000->3000/tcp
usr0jy4jquns    nuuvify_node-exporter        global       1/1       lzocateli/node-exporter_alpine:latest  *:9100->9100/tcp
zc3qza0bxys7    nuuvify_prometheus           replicated   1/1       lzocateli/prometheus_alpine:latest     *:9090->9090/tcp
```
PS: Don't worry why nuuvify_mongo-init-replica service is down, it only executes one time to initialize the replica set. It will not stay running.


## Access Services in Browser

To access Prometheus interface on browser:
```bash
http://YOUR_IP:9090
```

To access AlertManager interface on browser:
```bash
http://YOUR_IP:9093
```

To access Grafana interface on browser:
```bash
http://YOUR_IP:3000
user: admin
passwd: nuuvify

To add plugs edit file nuuvify-monitor/grafana.config
GF_INSTALL_PLUGINS=plug1,plug2
Current plugs grafana-clock-panel,grafana-piechart-panel,camptocamp-prometheus-alertmanager-datasource,vonage-status-panel
```
Access the dashboards

To access Netdata interface on browser:
```bash
http://YOUR_IP:19999
```

To access Prometheus Node_exporter metrics on browser:
```bash
http://YOUR_IP:9100/metrics
```

Test if your alerts are ok:
```bash
# docker service rm nuuvify_node-exporter

Wait some seconds and you will see the integration works fine! Prometheus alerting the AlertManager that alert the Slack that shows it to you! It's so easy and that simple! :D
```


Create new alerts on Prometheus:
```bash
# nano config/prometheus/alert.rules
```
