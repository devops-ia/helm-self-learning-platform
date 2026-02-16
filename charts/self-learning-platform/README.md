# self-learning-platform

Is an interactive learning platform focused on deliberate practice through hands-on problem solving. It provides:

- 🖊 Interactive coding environment with Monaco Editor
- 💻 Simulated terminal for realistic execution flows
- 🧪 Deterministic validation engine with instant feedback
- 🗂 Extensible exercise system for Terraform, Kubernetes, Python, Golang, and more
- 🔐 Role-based authentication with OAuth, TOTP 2FA, and WebAuthn passkeys
- 📜 Audit logging and progress tracking

## Maintainers

| Name | Email | Url |
| ---- | ------ | --- |
| ialejandro | <hello@ialejandro.rocks> | <https://ialejandro.rocks> |

## Prerequisites

* Helm 3+

## Add repository

```console
helm repo add self-learning-platform https://devops-ia.github.io/helm-self-learning-platform
helm repo update
```

## Install Helm chart (repository mode)

```console
helm install [RELEASE_NAME] self-learning-platform/self-learning-platform
```

This install all the Kubernetes components associated with the chart and creates the release.

_See [helm install](https://helm.sh/docs/helm/helm_install/) for command documentation._

## Install Helm chart (OCI mode)

Charts are also available in OCI format. The list of available charts can be found [here](https://github.com/devops-ia/helm-self-learning-platform/pkgs/container/helm-self-learning-platform%2Fself-learning-platform).

```console
helm install [RELEASE_NAME] oci://ghcr.io/devops-ia/helm-self-learning-platform/self-learning-platform --version=[version]
```

## Uninstall Helm chart

```console
helm uninstall [RELEASE_NAME]
```

This removes all the Kubernetes components associated with the chart and deletes the release.

_See [helm uninstall](https://helm.sh/docs/helm/helm_uninstall/) for command documentation._

## self-learning-platform

* [Environment configuration](./docs/environment.md)
* [Production-ready](./docs/production.md)

## Basic installation and examples

See [basic installation](docs/configuration.md), [clustering installation](docs/configuration-clustering-mode.md) and [examples](docs/examples.md).

## Configuration

See [Customizing the chart before installing](https://helm.sh/docs/intro/using_helm/#customizing-the-chart-before-installing). To see all configurable options with comments:

```console
helm show values self-learning-platform/self-learning-platform
```

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| affinity | object | `{}` | Affinity rules |
| args | list | `[]` | Configure container args |
| autoscaling | object | `{"behavior":{"scaleDown":{"policies":[{"periodSeconds":15,"type":"Percent","value":50},{"periodSeconds":60,"type":"Pods","value":1}],"selectPolicy":"Min","stabilizationWindowSeconds":300},"scaleUp":{"policies":[{"periodSeconds":15,"type":"Percent","value":100},{"periodSeconds":60,"type":"Pods","value":2}],"selectPolicy":"Max","stabilizationWindowSeconds":0}},"enabled":false,"maxReplicas":10,"minReplicas":2,"targetCPUUtilizationPercentage":80,"targetMemoryUtilizationPercentage":80}` | Horizontal Pod Autoscaler |
| autoscaling.behavior | object | `{"scaleDown":{"policies":[{"periodSeconds":15,"type":"Percent","value":50},{"periodSeconds":60,"type":"Pods","value":1}],"selectPolicy":"Min","stabilizationWindowSeconds":300},"scaleUp":{"policies":[{"periodSeconds":15,"type":"Percent","value":100},{"periodSeconds":60,"type":"Pods","value":2}],"selectPolicy":"Max","stabilizationWindowSeconds":0}}` | HPA scaling behavior |
| command | list | `[]` | Configure container command |
| configMaps | list | `[]` | ConfigMap values to create configuration files |
| dnsConfig | object | `{}` | DNS configuration |
| dnsPolicy | string | `"ClusterFirst"` | DNS policy |
| env | object | `{"HOST":"0.0.0.0","NODE_ENV":"production","PORT":"3000"}` | Environment variables to configure application |
| envFromConfigMap | object | `{}` | Variables from configMap |
| envFromFiles | list | `[]` | Load all variables from files |
| envFromSecrets | object | `{}` | Variables from secrets |
| fullnameOverride | string | `""` | String to fully override self-learning-platform.fullname template |
| global | object | `{"imagePullSecrets":[],"imageRegistry":""}` | Global section contains configuration options that are applied to all services |
| global.imagePullSecrets | list | `[]` | Specifies the secrets to use for pulling images from private registries |
| global.imageRegistry | string | `""` | Specifies the registry to pull images from. Leave empty for the default registry |
| image | object | `{"pullPolicy":"IfNotPresent","repository":"devopsiaci/self-learning-platform","tag":""}` | Image registry configuration |
| image.pullPolicy | string | `"IfNotPresent"` | Pull policy for the image |
| image.repository | string | `"devopsiaci/self-learning-platform"` | Repository of the image |
| image.tag | string | `""` | Overrides the image tag whose default is the chart appVersion |
| imagePullSecrets | list | `[]` | Specifies the secrets to use for pulling images from private registries |
| ingress | object | `{"annotations":{},"className":"","enabled":false,"hosts":[{"host":"self-learning-platform.local","paths":[{"path":"/","pathType":"Prefix"}]}],"tls":[]}` | Ingress configuration |
| initContainers | list | `[]` | Configure init containers |
| initJob | object | `{"backoffLimit":3,"enabled":true,"resources":{"limits":{"memory":"512Mi"},"requests":{"cpu":"100m","memory":"256Mi"}},"ttlSecondsAfterFinished":300}` | Init job configuration (runs db:seed on install/upgrade) |
| initJob.backoffLimit | int | `3` | Number of retries before marking job as failed |
| initJob.enabled | bool | `true` | Enable init job |
| initJob.resources | object | `{"limits":{"memory":"512Mi"},"requests":{"cpu":"100m","memory":"256Mi"}}` | Resource limits for init job |
| initJob.ttlSecondsAfterFinished | int | `300` | Seconds after job finishes before deletion |
| lifecycle | object | `{}` | Lifecycle hooks |
| livenessProbe | object | `{"enabled":true,"failureThreshold":3,"httpGet":{"path":"/healthz","port":"http"},"initialDelaySeconds":15,"periodSeconds":20,"successThreshold":1,"timeoutSeconds":5}` | Liveness probe configuration |
| livenessProbeCustom | object | `{}` | Custom liveness probe |
| nameOverride | string | `""` | String to partially override self-learning-platform.fullname template |
| networkPolicy | object | `{"egress":[],"enabled":false,"ingress":[],"policyTypes":[]}` | NetworkPolicy configuration |
| networkPolicy.enabled | bool | `false` | Enable or disable NetworkPolicy |
| networkPolicy.policyTypes | list | `[]` | Policy types |
| nodeSelector | object | `{}` | Node selector |
| persistence | object | `{"accessMode":"ReadWriteOnce","annotations":{},"enabled":false,"existingClaim":"","size":"1Gi","storageClassName":""}` | Persistence configuration for SQLite database |
| persistence.accessMode | string | `"ReadWriteOnce"` | Access mode |
| persistence.annotations | object | `{}` | Annotations for PVC |
| persistence.enabled | bool | `false` | Enable persistent volume for database |
| persistence.existingClaim | string | `""` | Use existing PVC |
| persistence.size | string | `"1Gi"` | Storage size |
| persistence.storageClassName | string | `""` | Storage class name |
| podAnnotations | object | `{}` | Pod annotations |
| podDisruptionBudget | object | `{"enabled":false,"maxUnavailable":1}` | Pod Disruption Budget |
| podLabels | object | `{}` | Pod labels |
| podSecurityContext | object | `{"fsGroup":1000,"seccompProfile":{"type":"RuntimeDefault"}}` | Pod security context |
| readinessProbe | object | `{"enabled":true,"failureThreshold":3,"httpGet":{"path":"/healthz","port":"http"},"initialDelaySeconds":5,"periodSeconds":10,"successThreshold":1,"timeoutSeconds":1}` | Readiness probe configuration |
| readinessProbeCustom | object | `{}` | Custom readiness probe |
| replicaCount | int | `2` | Number of replicas for the service |
| resources | object | `{"limits":{"memory":"1Gi"},"requests":{"cpu":"200m","memory":"512Mi"}}` | Resource limits and requests |
| secrets | list | `[]` | Secrets values to create credentials |
| securityContext | object | `{"allowPrivilegeEscalation":false,"capabilities":{"drop":["ALL"]},"readOnlyRootFilesystem":true,"runAsGroup":1000,"runAsNonRoot":true,"runAsUser":1000}` | Container security context |
| service | object | `{"annotations":{},"appProtocol":"HTTP","extraPorts":[],"labels":{},"port":80,"portName":"http","protocol":"TCP","targetPort":3000,"type":"ClusterIP"}` | Kubernetes service configuration |
| service.annotations | object | `{}` | Annotations for the service |
| service.appProtocol | string | `"HTTP"` | Application protocol |
| service.extraPorts | list | `[]` | Extra service ports |
| service.labels | object | `{}` | Additional labels for the service |
| service.port | int | `80` | Service port |
| service.portName | string | `"http"` | Name for the service port |
| service.protocol | string | `"TCP"` | Protocol for the service port |
| service.targetPort | int | `3000` | Pod target port |
| service.type | string | `"ClusterIP"` | Service type |
| serviceAccount | object | `{"annotations":{},"automountServiceAccountToken":false,"create":true,"name":""}` | ServiceAccount configuration |
| serviceAccount.annotations | object | `{}` | Annotations to add to the service account |
| serviceAccount.automountServiceAccountToken | bool | `false` | Disable automatic mounting of service account token |
| serviceAccount.create | bool | `true` | Specifies whether a service account should be created |
| serviceAccount.name | string | `""` | Name of the service account to use |
| serviceMonitor | object | `{"enabled":false,"interval":"30s","metricRelabelings":[],"relabelings":[],"scrapeTimeout":"10s"}` | ServiceMonitor for Prometheus Operator |
| serviceMonitor.enabled | bool | `false` | Enable ServiceMonitor |
| serviceMonitor.interval | string | `"30s"` | Scrape interval |
| serviceMonitor.metricRelabelings | list | `[]` | Metric relabeling |
| serviceMonitor.relabelings | list | `[]` | Relabeling |
| serviceMonitor.scrapeTimeout | string | `"10s"` | Scrape timeout |
| startupProbe | object | `{"enabled":true,"failureThreshold":30,"httpGet":{"path":"/healthz","port":"http"},"initialDelaySeconds":0,"periodSeconds":10,"successThreshold":1,"timeoutSeconds":1}` | Startup probe configuration |
| startupProbeCustom | object | `{}` | Custom startup probe |
| strategy | object | `{"rollingUpdate":{"maxSurge":1,"maxUnavailable":0},"type":"RollingUpdate"}` | Configure deployment strategy |
| terminationGracePeriodSeconds | int | `30` | Termination grace period in seconds |
| tolerations | list | `[]` | Tolerations |
| topologySpreadConstraints | list | `[]` | Topology spread constraints |
| volumeMounts | list | `[{"mountPath":"/tmp","name":"tmp"},{"mountPath":"/app/data","name":"data"}]` | Additional volume mounts |
| volumes | list | `[{"emptyDir":{},"name":"tmp"},{"emptyDir":{},"name":"data"}]` | Additional volumes |
