# 📡 OpenTelemetry Demo (Fork) – Cloud-Native Observability on EKS

> 🛠️ Forked from open-telemetry/opentelemetry-demo
> 
> 
> 🎓 *Graduate Course Project*: ENPM818N – Network Security (Spring 2025, UMD)
> 
> 🧪 *Focus*: Secure deployment, observability, and automation in Kubernetes/EKS environments.
> 

---

## 🧭 1. About This Fork

This repository was forked to study and implement full-stack *cloud-native observability* in a Kubernetes environment using AWS EKS.

It extends the official OpenTelemetry demo by:

- Automating EKS deployment with `eksctl`
- Securing services using IAM roles and TLS
- Configuring observability with Prometheus + Grafana
- Debugging real-world networking and build issues

---

## 🛠️ 2. Key Customizations

| Category | Modification |
| --- | --- |
| Infrastructure | Created managed EKS cluster with `--managed` node groups |
| IAM & Security | Applied least-privilege IAM roles, port restrictions, TLS |
| Observability | Installed Prometheus + Grafana via Helm, split ConfigMaps |
| Debugging | Solved gRPC ECONNREFUSED, Gradle `grpc-netty` dependency issue |
| Deployment | Helm + `kubectl apply` approach with namespace isolation |
| Docs | This README + full experimentation logs (see `/docs`) |

---

## 🧪 3. Experiment Workflow

### 🔧 Local Setup

- Docker, docker-compose, kind installation
- `docker-compose up -d` for local microservice validation
- Port-forwarding and internal service tests

### ☁️ EC2 (Standalone)

- Deployed the app on Ubuntu EC2 with docker-compose
- Verified app at `http://<EC2-IP>:8080`

### ☁️ AWS EKS

- Created EKS cluster using `eksctl --managed`
- Deployed OpenTelemetry YAML and kube-prometheus-stack Helm chart
- Configured port-forwarding, validated pod status, Grafana UI

---

## 📈 4. Observability & Monitoring

- Prometheus collected pod-level and service metrics
- Grafana dashboards showed trace and span data from OpenTelemetry Collector
- Helm values tweaked to avoid ConfigMap annotation limit (64KB)

---

## 🔐 5. Security Implementation

- IAM Role setup for EC2 and EKS interaction
- `aws sts get-caller-identity` used to confirm role binding
- Services limited via `ClusterIP`, port-forwarded securely
- TLS applied for internal communication

---

## 📚 6. Mapping to ENPM693 Topics

| Course Concept | Real Implementation |
| --- | --- |
| IAM & Access Control | Role assignment, STS validation |
| Secure Service Exposure | Internal DNS + TLS + port restrictions |
| Network Threat Modeling | Exposed service audit, traffic binding fixes |
| gRPC & Protocol Debugging | ECONNREFUSED fix, build failures, curl test |
| DevSecOps Automation | Cluster creation + deployment via `eksctl` and Helm |

---

## 💡 7. What I Learned

> This project taught me that cloud security is architectural, not just code-deep.
> 
- The `-managed` flag in `eksctl` simplified infrastructure ops and allowed me to focus on application-level security.
- Observability wasn't just about installing Prometheus—it was about understanding *what to monitor, and why.*
- Debugging container-to-container gRPC errors required visibility across layers: logs, healthchecks, DNS, builds, bindings.
- IAM misconfigurations are subtle but critical—roles, policies, and STS trust must all align.

---

## 📂 8. Repository Structure

```ebnf
.
├── README.md                 
├── README_official.md        
├── docs/
│   ├── setup_local.md
│   ├── setup_eks.md
│   └── troubleshooting.md
```

---

## ✅ 9. Full Issue Summary

| Category | ❌ Issue | 🧠 Root Cause | 🔧 Solution | ✅ Result |
| --- | --- | --- | --- | --- |
| IAM Role | `no EC2 IMDS role found` | No role attached | Attach role with EKS permissions | IAM verified with STS |
| ConfigMap Limit | Grafana dashboards missing | Size > 64KB | Split into multiple ConfigMaps | Dashboards loaded |
| gRPC Failure | ECONNREFUSED on ad service | Wrong port bind + missing dep | Add `grpc-netty`, bind to `0.0.0.0` | Tracing enabled |
| AWS CLI | CLI v1 incompatible with EKS | Old version | Install AWS CLI v2 | Cluster created |
| Service Access | No external access | ClusterIP + blocked port | Add port-forward + SG inbound | Services accessible |

---

## 📝 10. License

This repository is a fork of the OpenTelemetry Demo, licensed under the Apache 2.0 License.

---

Thank you!
