# faasd - serverless with containerd and CNI  🐳

[![Build Status](https://travis-ci.com/openfaas/faasd.svg?branch=master)](https://travis-ci.com/openfaas/faasd)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![OpenFaaS](https://img.shields.io/badge/openfaas-serverless-blue.svg)](https://www.openfaas.com)

faasd is the same OpenFaaS experience and ecosystem, but without Kubernetes. Functions and microservices can be deployed anywhere with reduced overheads whilst retaining the portability of containers and cloud-native tooling.

## About faasd

* is a single Golang binary
* can be set-up and left alone to run your applications
* is multi-arch, so works on Intel `x86_64` and ARM out the box
* uses the same core components and ecosystem of OpenFaaS

![demo](https://pbs.twimg.com/media/EPNQz00W4AEwDxM?format=jpg&name=small)

> Demo of faasd running in KVM

## What does faasd deploy?

* faasd - itself, and its [faas-provider](https://github.com/openfaas/faas-provider) for containerd
* [Prometheus](https://github.com/prometheus/prometheus)
* [OpenFaaS Gateway & UI](https://github.com/openfaas/faas/tree/master/gateway)
* [OpenFaaS queue-worker for NATS](https://github.com/openfaas/nats-queue-worker)
* [NATS](https://nats.io) for asynchronous processing and queues

You'll also need:

* [CNI](https://github.com/containernetworking/plugins)
* [containerd](https://github.com/containerd/containerd)
* [runc](https://github.com/opencontainers/runc)

You can use the standard [faas-cli](https://github.com/openfaas/faas-cli) along with pre-packaged functions from *the Function Store*, or build your own using any OpenFaaS template.

## Tutorials

### Get started on DigitalOcean or with cloud-init

* [Build a Serverless appliance with cloud-init and faasd](https://blog.alexellis.io/deploy-serverless-faasd-with-cloud-init/)

### Run locally on MacOS, Linux, or Windows with Multipass.run

* [Get up and running with your own faasd installation on your Mac/Ubuntu or Windows with cloud-config](https://gist.github.com/alexellis/6d297e678c9243d326c151028a3ad7b9)

### Get started on armhf / Raspberry Pi

You can run this tutorial on your Raspberry Pi, or adapt the steps for a regular Linux VM/VPS host.

* [faasd - lightweight Serverless for your Raspberry Pi](https://blog.alexellis.io/faasd-for-lightweight-serverless/)

### Using private repos / registries

To use private image repos, `~/.docker/config.json` needs to be copied to `/var/lib/faasd/.docker/config.json`.

If you'd like to set up your own private registry, [see this tutorial](https://blog.alexellis.io/get-a-tls-enabled-docker-registry-in-5-minutes/).

### Manual / developer instructions

See [here for manual / developer instructions](docs/DEV.md)

## Backlog

### Supported operations

* `faas login`
* `faas up`
* `faas list`
* `faas describe` 
* `faas deploy --update=true --replace=false`
* `faas invoke --async`
* `faas invoke`
* `faas rm`
* `faas store list/deploy/inspect`
* `faas version`
* `faas namespace`
* `faas secret`

Scale from and to zero is also supported. On a Dell XPS with a small, pre-pulled image unpausing an existing task took 0.19s and starting a task for a killed function took 0.39s. There may be further optimizations to be gained.

Other operations are pending development in the provider such as:

* `faas logs` - to stream logs on-demand for a known function
* `faas auth` - for the OAuth2 and OIDC integration

## Todo

Pending:

* [ ] Add support for using container images in third-party public registries
* [ ] Add support for using container images in private third-party registries
* [ ] Monitor and restart any of the core components at runtime if the container stops
* [ ] Bundle/package/automate installation of containerd - [see bootstrap from k3s](https://github.com/rancher/k3s)
* [ ] Provide ufw rules / example for blocking access to everything but a reverse proxy to the gateway container
* [ ] Provide [simple Caddyfile example](https://blog.alexellis.io/https-inlets-local-endpoints/) in the README showing how to expose the faasd proxy on port 80/443 with TLS

Done:

* [x] Provide a cloud-config.txt file for automated deployments of `faasd`
* [x] Inject / manage IPs between core components for service to service communication - i.e. so Prometheus can scrape the OpenFaaS gateway - done via `/etc/hosts` mount
* [x] Add queue-worker and NATS
* [x] Create faasd.service and faasd-provider.service
* [x] Self-install / create systemd service via `faasd install`
* [x] Restart containers upon restart of faasd
* [x] Clear / remove containers and tasks with SIGTERM / SIGINT
* [x] Determine armhf/arm64 containers to run for gateway
* [x] Configure `basic_auth` to protect the OpenFaaS gateway and faasd-provider HTTP API
* [x] Setup custom working directory for faasd `/var/lib/faasd/`
* [x] Use CNI to create network namespaces and adapters

