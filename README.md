# Monitoring stack template

A docker-compose including an entire stack of application and infrastructure monitoring, meant to be used as a point of reference.

Includes:
* Demo application - Nginx (official image)
* Prometheus (official image)
* Grafana (official image)
* Example exporters - nginx, digitalocean (community)

Stack is composed from official images. Exporters are community images.

## How to Run
```
$ docker-compose up
```

Grafana: `http://localhost:3000` (default credentials are admin/admin)
Prometheus: `http://localhost:9090`

## Notes

### Digital Ocean
To use the digitalocean exporter, please provide API key from the digitalocean console in `docker-compose.yml`

### Docker metrics
The stack also scrapes docker itself for relevant metrics. The metrics needs to be exposed in the docker daemon configuration.

Add the following to the daemon configuration:
```
{
  "experimental": true,
  "metrics-addr": "0.0.0.0:50501"
}
```

On Mac: Docker -> Preferences -> Docker Engine
On Windows: Docker -> Settings -> Docker Engine
On Linux: `/etc/docker/daemon.json` (and restart the docker daemon)

Please note that on linux you'll need to revise `prom.yml` with the IP of the docker host (`host.docker.internal` is not support on linux as of yet)

### Exposed ports
Ports are exposed on all applications for clarity and debugging purposes. On production, metrics ports should only be exposed within the internal docker network

## Development

Submitting pull requests and issues is welcome

## References
* [Prometheus - exporters](https://prometheus.io/docs/instrumenting/exporters/)
* [Grafana](https://grafana.com/docs/grafana/latest/installation/docker/)
* [Expose docker metrics](https://docs.docker.com/config/daemon/prometheus/)
* [Nginx status](http://nginx.org/en/docs/http/ngx_http_stub_status_module.html)
* [Target discovery](https://github.com/prometheus/prometheus/tree/master/discovery) - yet to be implemented here. Running on swarm or [K8s](https://prometheus.io/docs/prometheus/latest/configuration/configuration/#kubernetes_sd_config) will require to be implemented. Please read [Configuration](https://prometheus.io/docs/prometheus/latest/configuration/configuration) from Prometheus docs