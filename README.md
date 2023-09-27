# Helm Chart for TeamCity
This is a simple chart that stands up a single Server and Agent.
By default, it uses EmptyDir for Volumes and Ingress is disabled.

## Key Values
- agentReplicaCount: The number of Agents that will be created (default: 1)
- pvc.data: A PVC name for the Data directory on the Server (default: "").
- pvc.existingDataPvc: An existing PVC name for the Data directory on the Server (default: ""). Overrides `pvc.data`
- pvc.logs: A PVC name for the Logs directory on the Server (default: "").
- pvc.config: A PVC name for the configuration on the Agent (default: "").
- ingress.enabled: Set to true to enable Ingress
- ingress.hosts: Array of Host entries for Ingress
- teamcity.server.env: Environment properties that will be passed to TeamCity Server container

# Install
- Add Coda Charts Helm Repo

  `helm repo add coda-charts https://codaglobal.github.io/coda-charts/`

- Install with Helm

  `helm install teamcity coda-charts/teamcity`

# Example of values.yaml
```yaml
agentReplicaCount: 2

image:
  server:
    repository: jetbrains/teamcity-server
  agent:
    repository: jetbrains/teamcity-agent
  tag: "latest"
  pullPolicy: IfNotPresent

pvc:
  existingDataPvc: "my-teamcity-pvc"
  config: ""
  logs: ""

serviceAccount:
  create: true
  annotations: {}
  name: ""

podAnnotations: {}

podSecurityContext: {}

securityContext: {}

service:
  int_type: ClusterIP
  ext_type: NodePort
  port: 8111

ingress:
  enabled: true
  annotations:
    cert-manager.io/cluster-issuer: lets-encrypt
    ingress.kubernetes.io/whitelist-source-range: "10.00.0.0/8"
  hosts:
    - host: build.my.io
      paths: ["/"]
  tls:
    - secretName: build.my.io-ingress-tls
      hosts:
        - build.my.io

resources: {}
  # limits:
  #   cpu: 100m
  #   memory: 128Mi
  # requests:
  #   cpu: 100m
#   memory: 128Mi

nodeSelector: {}

tolerations: []

affinity: {}

```