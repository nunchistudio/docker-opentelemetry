# OpenTelemetry stack on Docker

Docker environment for running an [OpenTelemetry](https://opentelemetry.io/)
stack locally using:

- [**Prometheus**](https://prometheus.io/) for metrics;
- [**Mimir**](https://grafana.com/oss/mimir/) as Prometheus long-term storage;
- [**Loki**](https://grafana.com/oss/loki/) for logs;
- [**Tempo**](https://grafana.com/oss/tempo/) for traces;
- [**Grafana Agent**](https://grafana.com/oss/agent/) as telemetry collector;
- [**Grafana**](https://grafana.com/oss/grafana/) for visualization;
- [**MinIO**](https://min.io/) as storage for Mimir, Loki, Tempo.

The goal is to provide the *simplest* and *lightest* setup possible for running
these components together. This means no authentication, no HA configuration, no
load-balancer, no TLS.

## Setup

Clone the repository with:
```sh
$ git clone git@github.com:nunchistudio/docker-opentelemetry.git
```

Then, start the Docker environment:
```sh
$ cd ./docker-opentelemetry
$ docker compose up -d
```

At this point you should have all containers running (truncated for better
visibility):
```sh
$ docker ps

IMAGE                    PORTS
grafana/agent:latest     0.0.0.0:7020-7021->7020-7021/tcp
grafana/grafana:latest   0.0.0.0:3000->3000/tcp
prom/prometheus:latest   0.0.0.0:9090->9090/tcp
grafana/mimir:latest     0.0.0.0:3300->3300/tcp, 8080/tcp
grafana/tempo:latest     0.0.0.0:3200-3201->3200-3201/tcp, 0.0.0.0:50397->7946/tcp
grafana/loki:latest      0.0.0.0:3100->3100/tcp, 0.0.0.0:50398->7946/tcp
minio/minio:latest       0.0.0.0:9000-9001->9000-9001/tcp
```

You can access the Grafana dashboard at <http://localhost:3000>. Datasources are
already configured to work with Mimir, Loki, and Tempo.

OpenTelemetry collector endpoint is exposed at `localhost:7021`.

## License

Repository licensed under the [MIT License](./LICENSE.md).
