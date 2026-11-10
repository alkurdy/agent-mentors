# Technology Selection Matrix & The 5-Axis Tradeoff Compass

Never memorize tools by brand name or marketing claims. Classify systems by **Access Pattern**, **Physical Constraints**, and **Tradeoff Vectors**.

---

## The 5-Axis Tradeoff Compass

When evaluating any two competing technologies, plot them on these 5 axes:

```
          [Access Pattern]
          (Point Key vs Scan vs Log)
                     ▲
                     │
[Ops Burden] ◄───────┼───────► [Read / Write Ratio]
(Single Binary vs    │         (Read-Heavy vs Write-Heavy)
Distributed Cluster) │
                     ▼
          [Consistency & Durability]
          (ACID vs Eventual / RAM vs Disk)
```

1. **Access Pattern**: Single key lookup, complex relational JOIN, sequential stream replay, or columnar analytical scan?
2. **Read vs. Write Ratio**: 99% reads (cache aggressively) vs. 90% writes (append-only log, LSM-tree)?
3. **Consistency vs. Latency SLA**: Immediate ACID consensus (banking ledger) vs. Eventual consistency (social feed, telemetry)?
4. **Durability & Retention**: Volatile in-memory (sub-millisecond) vs. 7-day replay window vs. 10-year immutable audit log?
5. **Operational Burden**: Single self-contained binary (SQLite, Redis) vs. complex multi-node consensus cluster (Kafka, Cassandra, Kubernetes)?

---

## 1. Databases: Which Data Store When?

| Paradigm | Exemplars | Core Access Pattern | When to Choose | When to AVOID (The Trap) |
| :--- | :--- | :--- | :--- | :--- |
| **Relational (RDBMS)** | **PostgreSQL**, MySQL | B-Tree index; complex queries with multi-table JOINs; strict ACID transactions. | Default choice for 90% of apps. Financial records, e-commerce orders, user identity. | Massive unpartitioned writes ($>100\text{k/s}$) exceeding single-node NVMe IOPS. |
| **In-Memory Cache** | **Redis**, Memcached | Hash table in RAM; $O(1)$ sub-millisecond point lookups and atomic data structures. | Session caches, rate-limit counters, transient leaderboard state. | Using as primary source-of-truth database for business data without durability guarantees. |
| **Wide-Column / Distributed KV** | **Cassandra**, ScyllaDB, DynamoDB | LSM-trees; hash-partitioned keys; partition-key queries only (NO JOINs). | Enormous write volume ($>500\text{k/s}$) across multiple datacenters. IoT telemetry, chat history. | Needing cross-table joins, ad-hoc filters, or flexible SQL queries (anti-pattern). |
| **Document Store** | **MongoDB**, DocumentDB | B-Tree/WiredTiger; hierarchical JSON/BSON documents. | Rapid prototyping with polymorphic/unstructured schemas where nested objects match code classes. | Highly relational data requiring cross-collection joins and strict transactional isolation. |
| **Analytical Columnar (OLAP)** | **ClickHouse**, Snowflake, DuckDB | Column-oriented storage; vectorized execution; massive batch scans. | Running aggregations (`SUM`, `AVG`, `GROUP BY`) across 100M+ rows for analytics dashboards. | Point lookups by primary key or frequent single-row transactional updates/deletes. |
| **Full-Text Search** | **Elasticsearch**, Meilisearch | Inverted index; tokenized linguistic search and scoring. | Search bars, fuzzy string matching, log searching (ELK). | Primary transactional database (high update overhead, split-brain data loss risk). |

---

## 2. Messaging & Streaming: Queues vs. Logs

| Paradigm | Exemplars | Mechanics | The Deciding Question | When to Choose | The Trap |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Work Queue** | **RabbitMQ**, AWS SQS | **Smart Broker, Dumb Consumer**.<br>Broker tracks acks and deletes messages once processed. | *"Do independent workers need to consume tasks and delete them upon completion?"* | Background task execution (email sending, image resizing, invoice generation). | Using for event sourcing or replaying historical data (messages are destroyed after ack). |
| **Event Log** | **Apache Kafka**, Redpanda, AWS Kinesis | **Dumb Broker, Smart Consumer**.<br>Append-only immutable log on disk. Consumer tracks its own offset. Replay at will. | *"Do multiple disparate systems need to replay the same ordered history of events at their own pace?"* | Event-driven microservices, CDC (Change Data Capture), financial transaction ledgers. | Using as a simple work queue for 5 background jobs (massive operational overkill). |
| **Lightweight Stream** | **Redis Streams / PubSub** | In-memory stream with consumer groups. | *"Do you need fast streaming with minimal infrastructure overhead?"* | Real-time chat, telemetry ingestion where Redis is already in the stack. | Multi-terabyte long-term event retention. |

---

## 3. Compute & Runtime Architecture

| Layer | Exemplars | Latency to Deploy | Isolation Level | When to Choose | The Trap |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Bare Metal** | Physical Supermicro / Dell | Days / Weeks | Hardware physical | Zero-hypervisor latency, extreme database I/O, custom kernel/FPGA needs. | Rapid scaling or elastic workloads. |
| **Virtual Machines (VMs)** | VMware ESXi, KVM, AWS EC2 | Minutes | Hypervisor hardware virtualization | Legacy enterprise workloads, multi-tenant security boundaries, full OS customization. | Micro-task workloads with high packaging churn. |
| **Containers** | Docker, Podman | Seconds | Linux Namespaces + cgroups (OS-level) | Standard microservice packaging; consistent dev-to-prod parity. | Relying on containers alone for multi-tenant kernel security isolation. |
| **Orchestration** | **Kubernetes**, Nomad, ECS | Seconds | Dynamic cluster scheduler | Managing 10+ decoupled microservices requiring self-healing, rolling deploys, and service discovery. | Deploying for a single monolithic application or team of 2 engineers (Kubernetes tax). |
| **Serverless** | AWS Lambda, Cloudflare Workers | Milliseconds | Ephemeral microVM / V8 isolate | Bursting, spiky, event-driven workloads (S3 file upload trigger, webhook handler). | Long-running stateful computations, continuous background daemons (cold starts + cost spike). |

---

## 4. Storage Architecture: Block vs. File vs. Object

| Storage Type | Exemplars | Protocol | Access Mechanics | Primary Use Case |
| :--- | :--- | :--- | :--- | :--- |
| **Block Storage** | AWS EBS, SAN, Local NVMe | SCSI / NVMe / iSCSI | Raw sectors/blocks; formatted with filesystem (ext4, XFS). Mountable by **single** instance. | Databases (Postgres, MySQL), OS boot drives. Sub-millisecond IOPS. |
| **File Storage** | AWS EFS, NFS, SMB | NFSv4, CIFS | Hierarchical folder tree; shared concurrent read/write across **many** instances. | Shared configuration, CMS assets (WordPress wp-content), user home directories. |
| **Object Storage** | AWS S3, MinIO, GCS | HTTP REST (`GET`, `PUT`, `DELETE`) | Flat namespace with UUID keys; immutable blobs with metadata; infinite HTTP scale. | Media files, backups, logs, ML datasets. Highest durability, lowest cost per GB. |

---

## 5. Reverse Proxies, API Gateways & Service Meshes

| Layer | Exemplars | Position in Architecture | Core Responsibility |
| :--- | :--- | :--- | :--- |
| **Edge Reverse Proxy** | **NGINX**, HAProxy, Cloudflare | Perimeter / Edge (Internet $\to$ Datacenter) | TLS termination, static asset caching, DDoS mitigation, basic round-robin load balancing. |
| **API Gateway** | **Traefik**, Kong, AWS API GW | Boundary (Clients $\to$ Microservices) | JWT authentication, rate limiting, request routing, API schema validation, path rewrites. |
| **Service Mesh** | **Envoy**, Istio, Linkerd | Internal (Microservice $\to$ Microservice) | Mutual TLS (mTLS), internal service discovery, distributed tracing, circuit breaking between pods. |

---

## 6. Infrastructure as Code: Declarative vs. Procedural

| Tool Archetype | Exemplars | Paradigm | Primary Responsibility |
| :--- | :--- | :--- | :--- |
| **Infrastructure Provisioning** | **Terraform**, OpenTofu, Pulumi | **Declarative State**: Describes *what* infrastructure exists (VPCs, S3 buckets, RDS). | Spins up, modifies, and destroys cloud/virtual infrastructure. Maintains state graph. |
| **Configuration Management** | **Ansible**, SaltStack, Puppet | **Idempotent Configuration**: Describes *how* an OS is configured inside the VM/host. | Installs packages, templates config files, manages users, restarts systemd daemons. |
| **Image Baking** | **Packer**, Cloud-Init | **Immutable Golden Images**: Pre-bakes dependencies into VM templates/AMIs. | Eliminates long boot-time setup scripts; guarantees consistent OS baseline across cluster. |
