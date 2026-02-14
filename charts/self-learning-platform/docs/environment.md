## Parameters

### Global parameters

| Name                      | Description                                     | Value |
| ------------------------- | ----------------------------------------------- | ----- |
| `global.imageRegistry`    | Global Docker image registry                    | `""`  |
| `global.imagePullSecrets` | Global Docker registry secret names as an array | `[]`  |

### Common parameters

| Name                | Description                                        | Value |
| ------------------- | -------------------------------------------------- | ----- |
| `nameOverride`      | String to partially override fullname template     | `""`  |
| `fullnameOverride`  | String to fully override fullname template         | `""`  |
| `replicaCount`      | Number of replicas                                 | `2`   |

### Image parameters

| Name               | Description                                        | Value                         |
| ------------------ | -------------------------------------------------- | ----------------------------- |
| `image.repository` | Self Learning Platform image repository            | `self-learning-platform`      |
| `image.pullPolicy` | Image pull policy                                  | `IfNotPresent`                |
| `image.tag`        | Image tag (overrides chart appVersion)             | `""`                          |

### Application parameters

| Name                   | Description                                        | Value                    |
| ---------------------- | -------------------------------------------------- | ------------------------ |
| `env.NODE_ENV`         | Node environment                                   | `production`             |
| `env.PORT`             | Application HTTP port                              | `"3000"`                 |
| `env.HOST`             | Application bind address                           | `"0.0.0.0"`              |
| `env.BASE_URL`         | Public URL (required for OAuth/WebAuthn)           | `""`                     |
| `env.SESSION_SECRET`   | Session encryption key (min 32 chars, REQUIRED)    | `""`                     |
| `env.ADMIN_PASSWORD`   | Admin user password (REQUIRED)                     | `""`                     |
| `env.ADMIN_EMAIL`      | Admin user email                                   | `admin@devopslab.local`  |

**CRITICAL**: You MUST set `SESSION_SECRET` and `ADMIN_PASSWORD` via secrets:
```bash
kubectl create secret generic my-secrets \
  --from-literal=SESSION_SECRET=$(openssl rand -base64 32) \
  --from-literal=ADMIN_PASSWORD='YourPassword123!'
```

### Security parameters

| Name                                  | Description                                  | Value             |
| ------------------------------------- | -------------------------------------------- | ----------------- |
| `podSecurityContext.fsGroup`          | Group ID for the volumes                     | `1000`            |
| `podSecurityContext.seccompProfile`   | Seccomp profile                              | `RuntimeDefault`  |
| `securityContext.runAsNonRoot`        | Run containers as non-root user              | `true`            |
| `securityContext.runAsUser`           | User ID to run containers                    | `1000`            |
| `securityContext.runAsGroup`          | Group ID to run containers                   | `1000`            |
| `securityContext.readOnlyRootFilesystem` | Mount root filesystem as read-only        | `true`            |
| `securityContext.allowPrivilegeEscalation` | Allow privilege escalation            | `false`           |
| `securityContext.capabilities.drop`   | Linux capabilities to drop                   | `["ALL"]`         |

### Service parameters

| Name                   | Description                                  | Value       |
| ---------------------- | -------------------------------------------- | ----------- |
| `service.type`         | Kubernetes Service type                      | `ClusterIP` |
| `service.port`         | Service HTTP port                            | `80`        |
| `service.targetPort`   | Target port for the service                  | `3000`      |

### Persistence parameters

| Name                             | Description                                  | Value           |
| -------------------------------- | -------------------------------------------- | --------------- |
| `persistence.enabled`            | Enable persistent storage for SQLite DB      | `false`         |
| `persistence.storageClassName`   | Storage class name                           | `""`            |
| `persistence.accessMode`         | Access mode                                  | `ReadWriteOnce` |
| `persistence.size`               | Storage size                                 | `1Gi`           |
| `persistence.existingClaim`      | Use existing PVC                             | `""`            |

### Init job parameters

| Name                                  | Description                                  | Value   |
| ------------------------------------- | -------------------------------------------- | ------- |
| `initJob.enabled`                     | Enable database initialization job           | `true`  |
| `initJob.ttlSecondsAfterFinished`     | Job TTL after completion                     | `300`   |
| `initJob.backoffLimit`                | Number of retries before failure             | `3`     |

### Ingress parameters

| Name                  | Description                                      | Value                            |
| --------------------- | ------------------------------------------------ | -------------------------------- |
| `ingress.enabled`     | Enable ingress controller resource               | `false`                          |
| `ingress.className`   | IngressClass that will be used                   | `""`                             |
| `ingress.hosts`       | Ingress hosts configuration                      | `[{"host": "self-learning-platform.local"}]` |

### Resource parameters

| Name                        | Description                          | Value     |
| --------------------------- | ------------------------------------ | --------- |
| `resources.requests.cpu`    | CPU resource requests                | `200m`    |
| `resources.requests.memory` | Memory resource requests             | `512Mi`   |
| `resources.limits.memory`   | Memory resource limits               | `1Gi`     |

### Autoscaling parameters

| Name                                          | Description                                  | Value   |
| --------------------------------------------- | -------------------------------------------- | ------- |
| `autoscaling.enabled`                         | Enable Horizontal Pod Autoscaler             | `false` |
| `autoscaling.minReplicas`                     | Minimum number of replicas                   | `2`     |
| `autoscaling.maxReplicas`                     | Maximum number of replicas                   | `10`    |
| `autoscaling.targetCPUUtilizationPercentage`  | Target CPU utilization percentage            | `80`    |
| `autoscaling.targetMemoryUtilizationPercentage` | Target Memory utilization percentage      | `80`    |

### Monitoring parameters

| Name                       | Description                          | Value   |
| -------------------------- | ------------------------------------ | ------- |
| `serviceMonitor.enabled`   | Enable ServiceMonitor for Prometheus | `false` |
| `serviceMonitor.interval`  | Scrape interval                      | `30s`   |
