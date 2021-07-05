<p align="center"><img src="https://github.com/kedacore/keda/raw/main/images/logos/keda-word-colour.png" width="300"/></p>

<p style="font-size: 25px" align="center"><b>Kubernetes-based Event Driven Autoscaling - HTTP Add-On</b></p>
<p style="font-size: 25px" align="center">

The KEDA HTTP Add On allows Kubernetes users to automatically scale their HTTP servers up and down (including to/from zero) based on incoming HTTP traffic. Please see our [use cases document](./docs/use_cases.md) to learn more about how and why you would use this project.

| 🚧 **Project status: beta** 🚧|
|---------------------------------------------|
| ⚠ The HTTP add-on currently is in [beta](https://github.com/kedacore/http-add-on/releases/tag/v0.1.0). We can't yet recommend it for production usage because we are still developing and testing it. It may have "rough edges" including missing documentation, bugs and other issues. It is currently provided as-is without support.

## HTTP Autoscaling Made Simple

[KEDA](https://github.com/kedacore/keda) provides a reliable and well tested solution to scaling your workloads based on external events. The project supports a wide variety of [scalers](https://keda.sh/docs/2.2/scalers/) - sources of these events, in other words. These scalers are systems that produce precisely measurable events via an API.

KEDA does not, however, include an HTTP-based scaler out of the box for several reasons:

- The concept of an HTTP "event" is not well defined.
- There's no out-of-the-box single system that can provide an API to measure the current number of incoming HTTP events or requests.
- The infrastructure required to achieve these measurements is more complex and, in some cases, needs to be integrated into the HTTP routing system in the cluster (e.g. the ingress controller).

For these reasons, the KEDA core project has purposely not built generic HTTP-based scaling into the core.

This project, often called KEDA-HTTP, exists to provide that scaling. It is composed of simple, isolated components and includes an opinionated way to put them together.

## Installation

### Installing KEDA

Before you install any of these components, you need to install KEDA. Below are simplified instructions for doing so with [Helm](https://helm.sh), but if you need anything more customized, please see the [official KEDA deployment documentation](https://keda.sh/docs/2.0/deploy/). If you need to install Helm, refer to the [installation guide](https://helm.sh/docs/intro/install/).

>This document will rely on environment variables such as `${NAMESPACE}` to indicate a value you should customize and provide to the relevant command. In the above `helm install` command, `${NAMESPACE}` should be the namespace you'd like to install KEDA into. KEDA and the HTTP Addon provide scaling functionality to only one namespace per installation.

```console
helm repo add kedacore https://kedacore.github.io/charts
helm repo update
helm install keda kedacore/keda --namespace ${NAMESPACE} --set watchNamespace=${NAMESPACE}
```

### Install Helm Chart

This repository is within KEDA's default helm repository on [kedacore/charts](http://github.com/kedacore/charts), you can install it by running:

```console
helm repo add kedacore https://kedacore.github.io/charts
helm repo update
helm install http-add-on kedacore/keda-add-ons-http --create-namespace --namespace ${NAMESPACE}
```