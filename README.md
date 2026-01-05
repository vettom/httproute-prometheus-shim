[<img src="https://vettom-images.s3.eu-west-1.amazonaws.com/logo/vettom-banner.jpg">](https://vettom.pages.dev/)

# Promshim for Httproutes and Ingress

![Workflow Status](https://github.com/vettom/httproute_prometheus_shim/actions/workflows/publish_to_dockerhub.yml/badge.svg)
[![Latest Release](https://github.com/vettom/httproute_prometheus_shim/releases/latest/download/badge.svg)](https://github.com/bmg-audio-cloud-engineering/httproute-prometheus-shim/releases/latest)

Prometheus shim application to retrieve list of all Ingress, httproutes and present it in HTTP service Definition format for Prometheus to monitor.

Updates to the application will trigger following

- Get current tag from git repo
- Increment version tag and create package
- Push new version image to Dockerhub registry
- Update Helm chart to reflect version changes
- Package and upload chart to Dockerhub.

## Installing App

When a version of app is published, helm chart with same version is created(without v)
If release/tag/app version is v3.1.1, then the helm chart version will ve 3.1.1

## Access URL

App exposes ingress as well as HTTP routes

- http://<host>:<port>/ingress-sd # returns list of all ingress in HTTP service definition format
- http://<host>:<port>/httproutes-sd # returns list of all http routes in HTTP service definition format

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
    "targets": ["prometheus.radio.online", "grafana.radio.online"]
  }
]
```

## Requirements

App uses prometheus service account role permission to list Ingresses and Http routes. During deployment, you must ensure that the ServiceAccount used by Pod can list all Ingresses across all namespaces.

## Installation

Helm package is created and uploaded with correct version always

```bash
helm install httproute-prometheus-shim   --version 1.0.1
```
