# self-learning-platform Helm Chart

Build real technical capability through structured, hands-on problem solving — without infrastructure setup, external systems, or operational overhead. Designed for teams that measure performance by execution, not theory.

More info. [Self Learning Platform](https://github.com/devops-ia/self-learning-platform)

## Usage

Charts are available in:

* [Chart Repository](https://helm.sh/docs/topics/chart_repository/)
* [OCI Artifacts](https://helm.sh/docs/topics/registries/)

### Chart Repository

#### Add repository

```console
helm repo add self-learning-platform https://devops-ia.github.io/helm-self-learning-platform
helm repo update
```

#### Install Helm chart

```console
helm install [RELEASE_NAME] self-learning-platform/self-learning-platform
```

This install all the Kubernetes components associated with the chart and creates the release.

_See [helm install](https://helm.sh/docs/helm/helm_install/) for command documentation._

### OCI Registry

Charts are also available in OCI format. The list of available charts can be found [here](https://github.com/devops-ia/helm-self-learning-platform/pkgs/container/helm-self-learning-platform%2Fself-learning-platform).

#### Install Helm chart

```console
helm install [RELEASE_NAME] oci://ghcr.io/devops-ia/helm-self-learning-platform/self-learning-platform --version=[version]
```

## self-learning-platform chart

Can be found in [simple-krr-dashboard chart](https://github.com/devops-ia/helm-simple-krr-dashboard/tree/main/charts/simple-krr-dashboard).
