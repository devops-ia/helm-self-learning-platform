
## Production recommendations

For production deployments, follow these best practices:

### 1. Enable Persistent Storage

**CRITICAL**: Enable persistence to avoid data loss on pod restarts:

```yaml
persistence:
  enabled: true
  storageClassName: "fast-ssd"  # Use your storage class
  size: 10Gi
  accessMode: ReadWriteOnce
```

### 2. Configure Secrets Properly

**Never** set `SESSION_SECRET` or `ADMIN_PASSWORD` in values.yaml. Use Kubernetes secrets:

```bash
# Create secrets
kubectl create secret generic self-learning-platform-secrets \
  --from-literal=SESSION_SECRET=$(openssl rand -base64 32) \
  --from-literal=ADMIN_PASSWORD='SecureP@ssw0rd!2026'

# Reference in values.yaml
envFromSecrets:
  SESSION_SECRET:
    name: self-learning-platform-secrets
    key: SESSION_SECRET
  ADMIN_PASSWORD:
    name: self-learning-platform-secrets
    key: ADMIN_PASSWORD
```

### 3. Configure Application URL

Set the public URL for OAuth and WebAuthn:

```yaml
env:
  BASE_URL: "https://learning.example.com"
```

### 4. Enable Autoscaling

```yaml
autoscaling:
  enabled: true
  minReplicas: 2
  maxReplicas: 10
  targetCPUUtilizationPercentage: 70
  targetMemoryUtilizationPercentage: 80
```

### 5. Enable Pod Disruption Budget

```yaml
podDisruptionBudget:
  enabled: true
  maxUnavailable: 1
```

### 6. Configure Resource Limits

Adjust based on your user load:

```yaml
resources:
  requests:
    cpu: "500m"
    memory: "1Gi"
  limits:
    memory: "2Gi"
```

### 7. Enable Network Policies

```yaml
networkPolicy:
  enabled: true
  policyTypes:
    - Ingress
    - Egress
  ingress:
    - from:
      - namespaceSelector:
          matchLabels:
            name: ingress-nginx
      ports:
      - protocol: TCP
        port: 3000
  egress:
    # Allow DNS
    - to:
      - namespaceSelector:
          matchLabels:
            name: kube-system
      ports:
      - protocol: UDP
        port: 53
```

### 8. Configure Ingress with TLS

```yaml
ingress:
  enabled: true
  className: nginx
  annotations:
    cert-manager.io/cluster-issuer: letsencrypt-prod
    nginx.ingress.kubernetes.io/ssl-redirect: "true"
    nginx.ingress.kubernetes.io/force-ssl-redirect: "true"
  hosts:
    - host: learning.example.com
      paths:
        - path: /
          pathType: Prefix
  tls:
    - secretName: learning-platform-tls
      hosts:
        - learning.example.com
```

### 9. Optional: Configure OAuth Providers

For Google OAuth:

```yaml
envFromSecrets:
  OAUTH_GOOGLE_CLIENT_ID:
    name: oauth-secrets
    key: google-client-id
  OAUTH_GOOGLE_CLIENT_SECRET:
    name: oauth-secrets
    key: google-client-secret
env:
  OAUTH_GOOGLE_CALLBACK: /api/auth/oauth/google/callback
```

### 10. Optional: Configure SMTP

For email verification and password resets:

```yaml
envFromSecrets:
  SMTP_HOST:
    name: smtp-secrets
    key: host
  SMTP_USER:
    name: smtp-secrets
    key: user
  SMTP_PASS:
    name: smtp-secrets
    key: pass
env:
  SMTP_PORT: "587"
  SMTP_FROM: "noreply@example.com"
  SMTP_SECURE: "false"
```

## Complete Production Example

```yaml
# production-values.yaml
replicaCount: 3

env:
  BASE_URL: "https://learning.example.com"
  ADMIN_EMAIL: "admin@example.com"
  TOTP_ISSUER: "Example Learning Platform"
  NEXT_PUBLIC_REGISTRATION_ENABLED: "true"

envFromSecrets:
  SESSION_SECRET:
    name: platform-secrets
    key: session-secret
  ADMIN_PASSWORD:
    name: platform-secrets
    key: admin-password

persistence:
  enabled: true
  storageClassName: "fast-ssd"
  size: 10Gi

resources:
  requests:
    cpu: "500m"
    memory: "1Gi"
  limits:
    memory: "2Gi"

autoscaling:
  enabled: true
  minReplicas: 3
  maxReplicas: 10

podDisruptionBudget:
  enabled: true
  maxUnavailable: 1

ingress:
  enabled: true
  className: nginx
  annotations:
    cert-manager.io/cluster-issuer: letsencrypt-prod
  hosts:
    - host: learning.example.com
      paths:
        - path: /
          pathType: Prefix
  tls:
    - secretName: learning-platform-tls
      hosts:
        - learning.example.com

networkPolicy:
  enabled: true
  policyTypes:
    - Ingress
    - Egress

serviceMonitor:
  enabled: true
  interval: 30s
```
