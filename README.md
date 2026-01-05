[<img src="https://vettom-images.s3.eu-west-1.amazonaws.com/logo/vettom-banner.jpg">](https://vettom.pages.dev/Monitoring/promshim-ingress-gw/)

# Promshim for Httproutes and Ingress

![Workflow Status](https://github.com/vettom/httproute-prometheus-shim/actions/workflows/publish_to_dockerhub.yaml/badge.svg) [![Latest Release](https://img.shields.io/github/v/release/vettom/httproute-prometheus-shim)](https://github.com/vettom/httproute-prometheus-shim/releases/latest)

Prometheus shim application to retrieve list of all Ingress, httproutes and present it in HTTP service Definition format for Prometheus to monitor. This in turn can be used with tools like Blackbos-monitoring to automate monitoring of Ingress and Http routes.

## Installation

```bash
helm install httproute-prometheus-shim oci://registry-1.docker.io/dennysv/httproute-prometheus-shim --version 1.0.1
```

When a version of app is published, helm chart with same version is created(without v)
If release/tag/app version is v3.1.1, then the helm chart version will ve 3.1.1

## Access URL

There are 2 paths exposed by the app

- http://host:9113/ingress-sd # returns list of all ingress in HTTP service definition format
- http://host:9113/httproutes-sd # returns list of all http routes in HTTP service definition format

#### Sample output

Following is the sample output for httproutes-sd endpoint in Prometheus service discovery format. Monitoring of URL can be automated using this SD format.

```json
[
  {
    "labels": {
      "__meta_application": "httproutes",
      "__meta_origin": "eks_cluster",
      "__meta_source": "prom_shim_http_routes"
    },
    "targets": ["prometheus.vettom.online", "grafana.vettom.online"]
  }
]
```

## App design and workflow details

Prometheus shim application to retrieve list of all Ingress, httproutes and present it in HTTP service Definition format for Prometheus to monitor.

Updates to the application will trigger following

- Get current tag from git repo
- Increment version tag and create package
- Push new version image to Dockerhub registry
- Update Helm chart to reflect version changes
- Package and upload chart to Dockerhub.

## Requirements

App uses prometheus service account role permission to list Ingresses and Http routes. During deployment, you must ensure that the ServiceAccount used by Pod can list all Ingresses across all namespaces.
