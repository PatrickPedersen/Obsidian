#### Cadvisor
Is our container monitor. It currently monitors all our docker containers and send all the data to our Prometheus database.

#### Node-Exporter
Monitors the metal (I.E the server) and sends all data to Prometheus.

#### MariaDB-Exporter
Monitors the Database and sends all data to Prometheus.
This exporter requires a database user with the following permissions:
```
CREATE USER 'exporter'@'localhost' IDENTIFIED BY 'XXXXXXXX' WITH MAX_USER_CONNECTIONS 3;
GRANT PROCESS, REPLICATION CLIENT, SELECT ON *.* TO 'exporter'@'localhost';
```

#### TS-Prom
Monitors our Teamspeak server and sends all data to Prometheus.
Requires access to the Teamspeak query API.

#### Prometheus
Is our monitoring database. It currently stores all metrics and makes them available to Grafana for viewing.
Prometheus is configured through a config file located in "/var/docker-storage/prometheus"

#### Grafana
Is our monitoring dashboard, with a handful of pre-configured dashboards for monitoring, the [[Server]], our proxy [[Traefik]], Mariadb, and Teamspeak.