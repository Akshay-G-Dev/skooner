# skooner

Checking cluster health and changing Kubernetes resources often means switching between terminal commands and resource definitions. skooner puts live cluster status and resource management in a browser dashboard.

## Features

- Inspect namespaces, nodes, pods, replica sets, deployments, storage, and RBAC resources.
- Follow live cluster updates without refreshing the page.
- Track resource health through real-time charts when cluster metrics are available.
- Create, edit, delete, and scale resources, with inline API documentation for resource fields.
- Sign in with a Kubernetes service account token or a configured OpenID Connect provider.
- Use the dashboard from a desktop, tablet, or phone.

## Installation

You need a running Kubernetes cluster and `kubectl` configured to access it. Install metrics-server if you want resource metrics. OIDC is optional and requires cluster-side configuration.

skooner is written in JavaScript, but the documented installation runs in Kubernetes rather than through an npm CLI. The commands below use the donor project's deployment manifest and container image; they do not install a separately published standalone build.

Download the manifest:

```bash
curl -fsSLo kubernetes-skooner.yaml https://raw.githubusercontent.com/skooner-k8s/skooner/master/kubernetes-skooner.yaml
```

**Review the manifest before applying it.** Check its image reference, permissions, and namespace against your cluster's requirements.

```bash
kubectl apply -f kubernetes-skooner.yaml
```

## Usage

Forward the dashboard service to your machine:

```bash
kubectl -n kube-system port-forward service/skooner 8080:80
```

Open `http://localhost:8080` in your browser. Keep the port-forward command running while you use the dashboard.

For a limited-access example, create a service account and bind the built-in `view` role in the `default` namespace:

```bash
kubectl -n default create serviceaccount skooner-sa
kubectl -n default create rolebinding skooner-view --clusterrole=view --serviceaccount=default:skooner-sa
kubectl -n default create token skooner-sa
```

Paste the returned token into the login screen. This account can read most resources in `default`; it cannot edit them or inspect the entire cluster. Use RBAC roles appropriate to your work if you need broader access or resource changes. The token command requires a kubectl version that supports `create token`.

**Treat login tokens as credentials.** Do not commit them or expose the dashboard publicly without appropriate access controls and HTTPS.

Use port forwarding or a configured ingress rather than `kubectl proxy`. The API-server proxy does not preserve the Authorization header required by the dashboard.

<!-- seorepo:network:start -->
## More tools from our network

- [**octo-cli**](https://github.com/boostlyapi/octo-cli) — CLI на Go для публикации данных из PostgreSQL, MSSQL и MySQL через бессерверные веб-сервисы в OpenFaaS или Knative. Конфигурация сервиса задаётся в YAML.
- [**tool**](https://github.com/boostlyapi/tool) — CLI на Go для Apache Kafka: чтение и отправка сообщений в JSON, просмотр топиков, управление смещениями consumer groups и базовое администрирование.
- [**docker-ikev2-vpn-server**](https://github.com/Cabral-it/docker-ikev2-vpn-server) — Docker-based IKEv2 VPN server with shared-secret authentication and .mobileconfig generation for iOS and macOS, based on gaomd/docker-ikev2-vpn-server.
- [**akrobateo**](https://github.com/Dyooooou/akrobateo) — A Go Kubernetes operator that exposes LoadBalancer services through node hostPorts using DaemonSets and synchronizes their load balancer addresses.
- [**node-hnsn**](https://github.com/ghb13/node-hnsn) — A TypeScript node for decentralized document timestamping with Bitcoin and IPFS, with an HTTP API for signed work claims and file uploads.
- [**wg-access-server**](https://github.com/EngHirsi/wg-access-server) — A WireGuard VPN server with a web UI for user authentication, one-click device registration, QR-code setup, and configurable network isolation.
<!-- seorepo:network:end -->

## License

Apache-2.0. This project is based on [skooner-k8s/skooner](https://github.com/skooner-k8s/skooner), which is also licensed under Apache-2.0. Preserve applicable copyright notices, the license, and any upstream NOTICE file when redistributing the code, and mark modified files as required by the license.
