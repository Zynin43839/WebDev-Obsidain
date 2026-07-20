# Hands On Microservices With Kubernetes - Build deploy and manage scalable microservices on-kubernetes

> **Author:** Gigi Sayfan | **Pages:** 494 | **Publisher:** Packt Publishing

---

## ภาพรวม

หนังสือสอนสร้าง microservices ด้วย Go และ deploy บน Kubernetes แบบ hands-on ทุกขั้นตอน ผ่านโปรเจคตัวอย่างชื่อ Delinkcious (คล้าย Delicious จัดการ bookmarks) ครอบคลุมทั้ง dev-ops lifecycle ตั้งแต่ design, code, test, deploy, monitor จนถึง service mesh

---

## Chapter 1: Introduction to Kubernetes for Developers

Kubernetes คือ container orchestration platform ที่ deploy และจัดการ container จำนวนมากบน cluster ของเครื่อง (physical/virtual) มี origin จาก Google Borg system เปิดตัวปี 2014 ปัจจุบัน CNCF certifies ทุก 3 เดือน

**Architecture** แบ่งเป็น 2 ส่วนหลัก:

- **Control Plane**: API server (REST API กลาง), etcd (distributed key-value store เก็บ cluster state), Scheduler (จัด pod ลง node), Controller Manager (Node/Replication/Endpoints/ServiceAccount controllers)
- **Data Plane**: Kubelet (agent บนแต่ละ node คุยกับ API server), kube-proxy (networking proxy), Container Runtime (CRI interface รองรับ Docker, containerd)

**Kubernetes + Microservices = เข้ากันดีมาก** เพราะ:
- Packaging: แต่ละ service มี Dockerfile → image → Pod → ReplicaSet → Deployment
- Discovery: Service resource + DNS ให้ service ค้นหากันได้
- Security: Namespaces, ServiceAccounts, Secrets (encrypted at rest), Network Policies, RBAC
- Upgrading: Zero downtime ด้วย rolling updates, blue-green, canary deployments
- Scaling: Horizontal Pod Autoscaler (CPU/memory/custom metrics)
- Monitoring: Central logging, Prometheus metrics, cAdvisor

**Minikube** = single-node cluster สำหรับ dev local ติดตั้งง่าย พร้อม Helm (package manager) สำหรับ install chart ต่างๆ

---

## Chapter 2: Getting Started with Microservices

หลักการสำคัญ: **less is more** — แบ่ง system เป็น service เล็กๆ ที่ autonomous, มี interface/contract ชัดเจน, expose ผ่าน API (REST/gRPC)

**patterns สำคัญ**:

- **Autonomous microservice**: ไม่依赖 service อื่น, ไม่เปลี่ยน state ของ service อื่น
- **Client libraries**: ซ่อน complexity ของการ network call ไว้เบื้องหลัง interface เดียว
- **Uniformity vs Flexibility trade-off**: uniform (ภาษาเดียว, config เดียว) ง่ายต่อ management vs polyglot (ภาษาต่างกัน) ยืดหยุ่นกว่า ต้องหา sweet spot
- **Conway's Law**: โครงสร้าง team = โครงสร้าง system (Vertical / Horizontal / Matrix org)
- **Source control strategy**: Monorepo (ง่าย sync, ยาก incremental deploy), Multiple repos (boundary ชัด, ยาก cross-cutting changes), Hybrid
- **Data strategy**: One data store per microservice (ไม่ share DB), แต่ system ต้อง query ข้าม service ได้
  - **CQRS**: aggregate data ลง read-only store แยกจาก write store — เร็วแต่ซับซ้อน
  - **API Composition**: query แล้ว compose จากหลาย service — เบาแต่ dependency สูง
  - **Saga pattern**: จัดการ distributed transaction ด้วย compensating operations — ได้ atomicity แต่ violate isolation (eventual consistency)

**CAP theorem**: เลือกได้แค่ 2 จาก 3 (Consistency, Availability, Partition tolerance) — ระบบ microservices ส่วนใหญ่เป็น AP (eventual consistency)

---

## Chapter 3: Delinkcious — Sample Application

Delinkcious คือ bookmark manager มี 3 services: **LinkManager**, **UserManager**, **SocialGraphManager** เขียนด้วย Go ใช้ **Go kit** framework

**Go kit architecture** เป็น onion layers:
- **Service layer**: Business logic ล้วนๆ ไม่รู้จัก network/API
- **Endpoint layer**: แปลง service method เป็น generic endpoint (request/response)
- **Transport layer**: HTTP, gRPC, Thrift — encode/decode payload
- **Middleware**: Logging, metrics, rate limiting, auth, distributed tracing

**Directory structure**: `cmd/` (tools, e2e tests), `pkg/` (packages, unit tests), `svc/` (actual microservices + Dockerfile)

**Key pattern**: Interface-based design — store เป็น interface เดียวกับ manager → เปลี่ยน DB implementation ได้โดยไม่แก้ business logic (in-memory store สำหรับ testing, PostgreSQL สำหรับ production)

---

## Chapter 4: Setting Up the CI/CD Pipeline

**CI/CD pipeline** คือ automation จาก code commit → test → build image → deploy production

**ตัวเลือกที่ evaluating**:
- Jenkins X: ดีแต่ troubleshoot ยาก, ไม่ support monorepo
- Spinnaker: powerful แต่ซับซ้อน, ไม่ Kubernetes-specific
- **CircleCI** (เลือกสำหรับ CI): free, UI ดี, build Docker image + push registry
- **Argo CD** (เลือกสำหรับ CD): Kubernetes-aware, GitOps, UI สวย, runs in-cluster
- Tekton: Kubernetes-native แต่ยังใหม่

**GitOps**: code + config + manifests ทั้งหมดอยู่ใน Git, push change = trigger pipeline, rollback = revert commit

**CircleCI config**: ตั้ง build job ด้วย Go image + Postgres → checkout → get deps → run tests (Ginkgo) → build & push Docker images (via build.sh)

**Multi-stage Dockerfile**: Builder stage ด้วย golang image → compile static binary → copy ลง scratch image (size เล็กสุด, attack surface ต่ำสุด)

**Argo CD**: ติดตั้งใน namespace argocd, monitor Git repo, sync manifest กับ cluster state, รองรับ auto-sync, auto-prune, blue-green/canary

---

## Chapter 5: Configuring Microservices with Kubernetes

**Configuration types**: Convention over config, CLI flags, Environment variables, Config files (INI/XML/JSON/YAML/TOML)

**Dynamic configuration**: เปลี่ยน config ได้ runtime โดยไม่ restart service — ใช้ remote config store หรือ config service (etcd, Consul)

**Kubernetes ConfigMaps**: เก็บ key-value config แยกจาก code, inject เป็น env var หรือ volume mount, อัปเดตได้โดยไม่ rebuild image

**Kubernetes Custom Resources**: ขยาย Kubernetes API ด้วย CRD (Custom Resource Definition) เพื่อสร้าง domain-specific objects

**Service Discovery**: Kubernetes ให้ DNS-based discovery ฟรี — Pod ค้นหา service ผ่าน DNS name ใน cluster

---

## Chapter 6: Securing Microservices on Kubernetes

**Security principles**: Least privilege, Defense in depth, Minimize blast radius

**User accounts vs Service accounts**: User สำหรับคน (X.509, webhook), Service account สำหรับ pod (identity + RBAC binding)

**Kubernetes Secrets**: เก็บ credentials/passwords, encrypted at rest (etcd 1.7+), mount เป็น volume หรือ env var

**RBAC**: Role (permissions on resources) + Binding (user/service account → role) — มี Role (per namespace) และ ClusterRole (cluster-wide)

**Authentication → Authorization → Admission**: 3 ขั้นตอน kiểm soát access เข้า cluster

**Security best practices**:
- ใช้ minimal base images (scratch, Alpine)
- Scan image vulnerabilities
- Network Policies แยก traffic ระหว่าง namespaces
- Security contexts + Pod security policies
- Resource quotas จำกัด CPU/memory per namespace

---

## Chapter 7: Talking to the World — APIs and Load Balancers

**Kubernetes Service types**: ClusterIP (internal), NodePort, LoadBalancer

**East-west vs North-south traffic**: ใน cluster (service-to-service) vs นอก cluster (user-to-service)

**Ingress**: ทางเข้า cluster จากภายนอก, route traffic ตาม hostname/path

**gRPC**: Internal API ระหว่าง services, ใช้ Protocol Buffers, generated stubs + client libraries

**NATS**: Message queue สำหรับ event-driven communication แบบ loosely coupled — services publish/subscribe events โดยไม่ต้องรู้จักกัน

**Service Mesh**: เริ่ม introducet — abstraction layer สำหรับ cross-cutting concerns (traffic management, security, observability) โดยไม่ต้องแก้ application code

---

## Chapter 8: Working with Stateful Services

**Kubernetes storage model**:
- **Persistent Volumes (PV)**: Storage ที่ provision ไว้
- **Persistent Volume Claims (PVC)**: Pod ขอใช้ storage
- **Storage Classes**: Dynamic provisioning
- **CSI (Container Storage Interface)**: Standard สำหรับ storage plugins

**StatefulSets**: สำหรับ stateful workloads ที่ต้องการ:
- Stable network identity (pod-0, pod-1, ...)
- Ordered deployment/scaling
- Persistent storage per pod

**ตัวอย่าง**: Deploy Cassandra บน Kubernetes ด้วย StatefulSets, ใช้ local SSD สำหรับ performance สูง

**Relational databases on K8s**: ใช้ Deployment + Service (simple) หรือ StatefulSets (production) — ช่วย pod ค้นหา DB ผ่าน DNS

**Redis**: In-memory data store สำหรับ caching, message broker, event persistence

---

## Chapter 9: Running Serverless Tasks on Kubernetes

**Serverless on Kubernetes**: Functions-as-a-Service (FaaS) — deploy function โดยไม่ต้องจัดการ infrastructure

**Models**: Functions as code (zip upload) vs Functions as containers

**Nuclio**: Serverless framework สำหรับ K8s — deploy link-checker function ที่ trigger เมื่อมี new link

**Other frameworks**: Kubernetes Jobs/CronJobs, KNative, Fission, Kubeless, OpenFaas

---

## Chapter 10: Testing Microservices

**Unit testing**: Go testing, Ginkgo + Gomega framework, mocking สำหรับ external dependencies

**Integration testing**: Test ระหว่าง services จริง + DB จริง, initialize test database, run service locally

**Telepresence**: เชื่อม local dev environment เข้า cluster จริง — debug service ใน IDE ขณะที่ service อื่นอยู่ใน cluster

**Test isolation**: ใช้ test namespaces, test clusters (per developer หรือ dedicated for system tests)

**End-to-end testing**: Acceptance, regression, performance testing — ครอบคลุมทั้ง system

**Test data**: Synthetic data, manual data, production snapshots

---

## Chapter 11: Deploying Microservices

**Deployment strategies**:

- **Recreate**: ลบ pod เก่าทั้งหมด → deploy ใหม่ (downtime)
- **Rolling updates**: อัปเดต pod ทีละตัว (zero downtime, default K8s)
- **Blue-green**: Deploy version ใหม่ alongside เก่า → switch traffic ทีเดียว ( rollback ง่าย)
- **Canary**: ส่ง traffic ส่วนน้อยไป version ใหม่ก่อน → gradually increase (A/B testing)

**Rollback**: Kubernetes รองรับ rollback ทุก deployment strategy

**Versioning & Dependencies**: จัดการ API versions, cross-service dependencies, third-party dependencies

**Local development tools**: Ko (Go), Ksync (file sync), Draft, Skaffold (build+deploy cycle), Tilt (multi-service dev)

---

## Chapter 12: Monitoring, Logging, and Metrics

**Self-healing**: K8s restart crashed containers, reschedule pods เมื่อ node ตาย

**Autoscaling**: HPA (Horizontal Pod Autoscaler) based on CPU/memory/custom metrics, Cluster Autoscaler (เพิ่ม/ลด nodes), VPA (Vertical Pod Autoscaler)

**Resource provisioning**: Define container limits (CPU/memory), resource quotas per namespace

**Logging**: Central logging ด้วย logging agent per node, sidecar container หรือ direct shipping — รวม log ไว้ที่เดียว

**Metrics**: Prometheus (deploy in-cluster), custom metrics, Kubernetes Metrics API

**Alerting**: Prometheus Alertmanager — กำหนด severity levels, alert channels, ลด noisy alerts

**Distributed tracing**: Jaeger — trace request ข้าม multiple services ตั้งแต่ต้นจนจบ

---

## Chapter 13: Service Mesh — Working with Istio

**Service mesh คืออะไร**: Infrastructure layer ที่จัดการ cross-cutting concerns (traffic, security, observability) โดยไม่ต้องแก้ app code — แก้ปัญหา shared library approach ที่ hard to update ทุก service พร้อมกัน

**Istio architecture**:
- **Envoy proxy**: Sidecar ทุก pod — intercept traffic
- **Pilot**: Traffic management, routing rules
- **Mixer**: Policy enforcement, telemetry
- **Citadel**: Certificate management, mTLS
- **Galley**: Config validation

**Istio capabilities**: Traffic routing, load balancing, fault injection (testing), canary deployments, mTLS (mutual authentication), RBAC, policy enforcement, automatic metrics collection

**Alternatives**: Linkerd 2.0, Envoy standalone, HashiCorp Consul, AWS App Mesh

**Trade-off**: Istio เพิ่ม complexity + resource overhead — ควรใช้เมื่อ system ใหญ่พอที่ต้องการ

---

## Chapter 14: The Future of Microservices and Kubernetes

**Microservices trends**: vs Serverless functions (เลือกตาม use case), gRPC + gRPC-Web (better than REST), GraphQL (flexible queries), HTTP/3 (QUIC protocol)

**Kubernetes extensibility**: CRI (container runtime), CNI (networking), CSI (storage), Cloud Provider Interface, Service mesh integration

**Serverless on K8s**: KNative, รองรับ VM-level isolation (gVisor, Firecracker, Kata containers)

**Cluster management**: Autoscaling, Operators (custom controllers for complex apps), Federation (multi-cluster management)

---

## สรุปรวม

หนังสือเล่มนี้เป็นคู่มือ practial สำหรับทุกคนที่อยากสร้าง microservices ระบบจริงบน Kubernetes เนื้อหาครอบคลุม lifecycle ครบทุก phase: design (patterns, data strategy), implement (Go, Go kit), secure (RBAC, secrets, network policies), deploy (CI/CD, GitOps), operate (monitoring, tracing, autoscale), และ scale (service mesh, multi-cluster) — ทุก concept มี code example จริงจาก Delinkcious project ทำให้เข้าใจได้ลึกกว่าหนังสือ theory-only

---
