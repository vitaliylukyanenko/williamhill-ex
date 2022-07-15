# app Helm Chart

* Installs the web dashboarding system [app](http://app.org/)


_See [helm repo](https://helm.sh/docs/helm/helm_repo/) for command documentation._

## Installing the Chart

To install the chart with the release name `app`:

```console
helm install app .  --set API_KEY=TESTTOKEN
```

## Uninstalling the Chart

To uninstall/delete the release:

```console
helm delete app
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Upgrading an existing Release to a new major version

A major chart version change (like v1.2.3 -> v2.0.0) indicates that there is an
incompatible breaking change needing manual actions.

### To 4.0.0 (And 3.12.1)

This version requires Helm >= 2.12.0.

### To 5.0.0

You have to add --force to your helm upgrade command as the labels of the chart have changed.

### To 6.0.0

This version requires Helm >= 3.1.0.

## Configuration

| Parameter                                 | Description                                   | Default                                                 |
|-------------------------------------------|-----------------------------------------------|---------------------------------------------------------|
| `replicas`                                | Number of nodes                               | `1`                                                     |
| `podDisruptionBudget.minAvailable`        | Pod disruption minimum available              | `nil`                                                   |
| `podDisruptionBudget.maxUnavailable`      | Pod disruption maximum unavailable            | `nil`                                                   |
| `deploymentStrategy`                      | Deployment strategy                           | `{ "type": "RollingUpdate" }`                           |
| `livenessProbe`                           | Liveness Probe settings                       | `{ "httpGet": { "path": "/api/health", "port": 3000 } "initialDelaySeconds": 60, "timeoutSeconds": 30, "failureThreshold": 10 }` |
| `readinessProbe`                          | Readiness Probe settings                      | `{ "httpGet": { "path": "/api/health", "port": 3000 } }`|
| `securityContext`                         | Deployment securityContext                    | `{"runAsUser": 472, "runAsGroup": 472, "fsGroup": 472}`  |
| `priorityClassName`                       | Name of Priority Class to assign pods         | `nil`                                                   |
| `image.repository`                        | Image repository                              | `app/app`                                       |
| `image.tag`                               | Overrides the app image tag whose default is the chart appVersion (`Must be >= 5.0.0`) | ``                                                      |
| `image.sha`                               | Image sha (optional)                          | ``                                                      |
| `image.pullPolicy`                        | Image pull policy                             | `IfNotPresent`                                          |
| `image.pullSecrets`                       | Image pull secrets (can be templated)         | `[]`                                                    |
| `service.enabled`                         | Enable app service                        | `true`                                                  |
| `service.type`                            | Kubernetes service type                       | `ClusterIP`                                             |
| `service.port`                            | Kubernetes port where service is exposed      | `80`                                                    |
| `service.portName`                        | Name of the port on the service               | `service`                                               |
| `service.targetPort`                      | Internal service is port                      | `3000`                                                  |
| `service.nodePort`                        | Kubernetes service nodePort                   | `nil`                                                   |
| `service.annotations`                     | Service annotations (can be templated)        | `{}`                                                    |
| `service.labels`                          | Custom labels                                 | `{}`                                                    |
| `service.clusterIP`                       | internal cluster service IP                   | `nil`                                                   |
| `service.loadBalancerIP`                  | IP address to assign to load balancer (if supported) | `nil`                                            |
| `service.loadBalancerSourceRanges`        | list of IP CIDRs allowed access to lb (if supported) | `[]`                                             |
| `service.externalIPs`                     | service external IP addresses                 | `[]`                                                    |
| `headlessService`                         | Create a headless service                     | `false`                                                 |
| `extraExposePorts`                        | Additional service ports for sidecar containers| `[]`                                                   |
| `hostAliases`                             | adds rules to the pod's /etc/hosts            | `[]`                                                    |
| `ingress.enabled`                         | Enables Ingress                               | `false`                                                 |
| `ingress.annotations`                     | Ingress annotations (values are templated)    | `{}`                                                    |
| `ingress.labels`                          | Custom labels                                 | `{}`                                                    |
| `ingress.path`                            | Ingress accepted path                         | `/`                                                     |
| `ingress.pathType`                        | Ingress type of path                          | `Prefix`                                                |
| `ingress.hosts`                           | Ingress accepted hostnames                    | `["chart-example.local"]`                                                    |
| `ingress.extraPaths`                      | Ingress extra paths to prepend to every host configuration. Useful when configuring [custom actions with AWS ALB Ingress Controller](https://kubernetes-sigs.github.io/aws-alb-ingress-controller/guide/ingress/annotation/#actions). Requires `ingress.hosts` to have one or more host entries. | `[]`                                                    |
| `ingress.tls`                             | Ingress TLS configuration                     | `[]`                                                    |
| `resources`                               | CPU/Memory resource requests/limits           | `{}`                                                    |
| `nodeSelector`                            | Node labels for pod assignment                | `{}`                                                    |
| `tolerations`                             | Toleration labels for pod assignment          | `[]`                                                    |
| `affinity`                                | Affinity settings for pod assignment          | `{}`                                                    |
| `extraInitContainers`                     | Init containers to add to the app pod     | `{}`                                                    |
| `envValueFrom`                            | Environment variables from alternate sources. See the API docs on [EnvVarSource](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.17/#envvarsource-v1-core) for format details. Can be templated | `{}` |
| `envFromSecret`                           | Name of a Kubernetes secret (must be manually created in the same namespace) containing values to be added to the environment. Can be templated | `""` |
| `envFromSecrets`                          | List of Kubernetes secrets (must be manually created in the same namespace) containing values to be added to the environment. Can be templated | `[]` |
| `envFromConfigMaps`                       | List of Kubernetes ConfigMaps (must be manually created in the same namespace) containing values to be added to the environment. Can be templated | `[]` |
| `envRenderSecret`                         | Sensible environment variables passed to pods and stored as secret | `{}`                               |
| `enableServiceLinks`                      | Inject Kubernetes services as environment variables. | `true`                                           |
| `extraSecretMounts`                       | Additional app server secret mounts       | `[]`                                                    |
| `extraVolumeMounts`                       | Additional app server volume mounts       | `[]`                                                    |
| `extraConfigmapMounts`                    | Additional app server configMap volume mounts (values are templated) | `[]`                         |
| `extraEmptyDirMounts`                     | Additional app server emptyDir volume mounts | `[]`                                                 |
| `rbac.namespaced`                         | Creates Role and Rolebinding instead of the default ClusterRole and ClusteRoleBindings for the app instance  | `false` |
| `rbac.useExistingRole`                    | Set to a rolename to use existing role - skipping role creating - but still doing serviceaccount and rolebinding to the rolename set here. | `nil` |
| `rbac.pspEnabled`                         | Create PodSecurityPolicy (with `rbac.create`, grant roles permissions as well) | `true`                 |
| `rbac.pspUseAppArmor`                     | Enforce AppArmor in created PodSecurityPolicy (requires `rbac.pspEnabled`)  | `true`                    |
| `rbac.extraRoleRules`                     | Additional rules to add to the Role           | []                                                      |
| `rbac.extraClusterRoleRules`              | Additional rules to add to the ClusterRole    | []                                                      |
| `command`                     | Define command to be executed by app container at startup  | `nil`                                              |

