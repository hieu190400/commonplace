# Tổng kết K3s phase 1

## 1. Compute

### Dùng để chạy workloads

#### Đã triển khai

- Deployment-> ReplicaSet -> Pods (Stateless application)
- Statefulset -> Pod (Stateful application)
- DaemonSet -> Pod (Một Pod trên mỗi Node)

#### Chưa triển khai

- Job -> Pod (Batch task chạy một lần)
- CronJob -> Job -> Pod (Batch task chạy theo lịch)

#### Tính năng đã sử dụng

- Rolling Update (mở pod mới xong mới xóa pod cũ) 
- Resource Requests/Limits (Giới hạn CPU/RAM)

### Enterprise chưa làm thử

#### Scaling

- Horizontal Pod Autoscaler (HPA): Tự động tăng/giảm số lượng Pod theo CPU, Memory hoặc custom metrics.
- Vertical Pod Autoscaler (VPA): Tự động điều chỉnh Requests/Limits cho Pod.
- Cluster Autoscaler: Tự động thêm hoặc giảm số lượng Node khi cluster thiếu hoặc dư tài nguyên.

#### Scheduling

- Node Selector: Chạy Pod trên Node có label cụ thể.
- Node Affinity / Anti-Affinity: Quy định Pod nên hoặc không nên chạy trên Node nào.
- Pod Affinity / Anti-Affinity: Quy định Pod nên hoặc không nên chạy gần nhau.
- Topology Spread Constraints: Phân phối Pod đều giữa các Node, Rack hoặc Zone.

#### Availability

- PodDisruptionBudget (PDB): Đảm bảo luôn còn tối thiểu một số Pod hoạt động khi bảo trì hoặc nâng cấp Node.
- PriorityClass: Quy định Pod nào được ưu tiên khi thiếu tài nguyên.
- RuntimeClass: Chọn runtime khác nhau (runc, gVisor, Kata Containers...).

#### Specialized Workload

- GPU Scheduling: Phân phối workload AI/ML lên Node có GPU.

---

## 2. Networking

### Dùng để kết nối workloads và expose ứng dụng

#### Đã triển khai

- Traefik (Gateway Controller) 
- Gateway API (Chuẩn khai báo Gateway và Route)
- HTTPRoute (1 loại route của Gateway API)
- Service (Expose Pod trong Cluster)
- ServiceALB: (làm LoadBalancer)

#### Chưa triển khai

- NGINX Ingress Controller
- GRPCRoute
- TCPRoute
- TLSRoute
- cert-manager (Tự động cấp/gia hạn TLS)

### Enterprise thường bổ sung

#### Network Security

- NetworkPolicy: Kiểm soát Pod nào được phép giao tiếp với Pod khác.
- mTLS: Mã hóa và xác thực kết nối giữa các Service.
- WAF: Chặn các cuộc tấn công HTTP phổ biến (SQL Injection, XSS...).

#### Traffic Management

- Rate Limiting: Giới hạn số lượng request từ client.
- API Gateway: Xác thực, phân quyền, giới hạn tốc độ và quản lý API.
- Service Mesh: Quản lý giao tiếp giữa các Service (Traffic, Security, Observability).

#### High Availability

- Global Load Balancer: Phân phối traffic giữa nhiều Cluster hoặc nhiều Region.
- Multi-region Ingress: Cho phép ứng dụng hoạt động trên nhiều Region hoặc Datacenter.

---

## 3. Storage

### Dùng để lưu trữ dữ liệu cho workloads

#### Đã triển khai

- Longhorn (Distributed Block Storage cho Kubernetes)
- Persistent Volume Claim - PVC (Chuẩn khai báo yêu cầu cấp volume cho Pod)
- StorageClass (Định nghĩa cách tạo và quản lý volume. Có replica và backup)
- MinIO (bên ngoài cho backup và dữ liệu log.)
- Backup và snapshot longhorn.

#### Chưa triển khai

- External CSI Storage (NFS, Ceph, TrueNAS...). Kết nối Kubernetes với hệ thống lưu trữ bên ngoài Cluster.

### Enterprise thường bổ sung

#### Data Protection

- Disaster Recovery: Khôi phục dữ liệu khi Cluster hoặc Storage gặp sự cố.

#### High Availability

- Multi-zone Storage: Dữ liệu được phân tán giữa nhiều Zone hoặc Datacenter.

#### Security

- Encryption at Rest: Mã hóa dữ liệu khi lưu trên ổ đĩa.

---

## 4. Configuration

### Lưu trữ cấu hình/Secret của ứng dụng

#### Đã triển khai

- Helm Values (Quản lý giá trị cấu hình của Helm Chart)
- ConfigMap (Lưu cấu hình không nhạy cảm)
- Secret (Lưu dữ liệu nhạy cảm)
- External Secrets (Đồng bộ Secret từ nguồn bên ngoài)
- Vault (Quản lý Secret tập trung)

#### Chưa triển khai

- Secret Rotation (Tự động làm mới Secret)
- Secret Injection (Inject Secret trực tiếp vào Pod qua Vault Agent CSI/Sidecar)
- Dynamic Secret (Database Credential sinh động từ Vault)

### Enterprise thường bổ sung

#### Configuration

- Dynamic Config: Thay đổi cấu hình mà không cần deploy lại.
- Feature Flags: Bật/tắt tính năng theo môi trường hoặc người dùng.
- Config Audit: Theo dõi lịch sử thay đổi cấu hình.

#### Secret Management

- Secret Rotation: Tự động xoay vòng Secret.
- Secret Injection: Không lưu Secret trong Kubernetes Secret mà inject khi Pod khởi động.
- Dynamic Secret: Sinh Secret theo yêu cầu với thời gian sống ngắn.

--- 

## 5. Identity & Access

### Quản lý danh tính và phân quyền

#### Đã triển khai

- Chưa làm gì hết.

#### Chưa triển khai

- Vault Authentication
- OIDC
- SSO
- Authentik
- LDAP
- Fine-grained RBAC
- Audit Trail

### Enterprise thường bổ sung

#### Authentication

- OIDC: Đăng nhập bằng Identity Provider.
- SSO: Một tài khoản dùng cho nhiều hệ thống.
- LDAP/Active Directory: Quản lý người dùng tập trung.

#### Authorization

- Fine-grained RBAC: Phân quyền chi tiết theo Team, Namespace hoặc Resource.

#### Audit

- Audit Trail: Ghi lại ai đã đăng nhập và thực hiện thay đổi gì.

---

## 6. Observability

### Theo dõi trạng thái và hoạt động của workloads

#### Đã triển khai

- Prometheus (Thu thập Metrics)
- Grafana (Hiển thị Dashboard)
- Loki (Lưu Logs)
- Tempo (Lưu Traces)
- OpenTelemetry Collector (Thu thập và chuyển tiếp Telemetry)
- OpenTelemetry Operator (Tự động Instrumentation)
- Auto Instrumentation (NodeGoat)
- Metrics
- Logs
- Traces

#### Chưa triển khai

- Alertmanager
- Recording Rules
- SLO / SLA
- Synthetic Monitoring

### Enterprise thường bổ sung

#### Monitoring

- Alertmanager: Gửi cảnh báo khi hệ thống gặp sự cố.
- Recording Rules: Tính toán và lưu sẵn Metrics để truy vấn nhanh hơn.
- SLO / SLA: Đo lường độ tin cậy của dịch vụ.

#### Observability

- Synthetic Monitoring: Mô phỏng người dùng truy cập để kiểm tra dịch vụ.
- Incident Integration: Kết nối Alert với Slack, Teams, PagerDuty...

---

## 7. Security

### Bảo vệ Platform và Workloads

#### Đã triển khai

- Vault (Quản lý Secret)
- External Secrets (Đồng bộ Secret từ Vault)

#### Chưa triển khai

- Internal TLS / cert-manager
- NetworkPolicy
- Image Signing
- Vulnerability Scan
- Admission Controller
- Pod Security
- Runtime Detection

### Enterprise thường bổ sung

#### Container Security

- Image Signing: Xác minh Image được build từ nguồn tin cậy.
- Vulnerability Scan: Quét lỗ hổng của Image trước khi deploy.
- Runtime Detection: Phát hiện hành vi bất thường khi Container đang chạy.

#### Kubernetes Security

- NetworkPolicy: Kiểm soát Pod nào được phép giao tiếp với nhau.
- Admission Controller: Kiểm tra Policy trước khi Resource được tạo.
- Pod Security: Hạn chế Container chạy với quyền cao.

--- 

## 8. Delivery

### Triển khai và cập nhật ứng dụng

#### Đã triển khai

- GitOps (Quản lý hạ tầng bằng Git)
- ArgoCD (Đồng bộ Cluster từ Git)
- Helm (Đóng gói ứng dụng)
- App of Apps (Bootstrap nhiều ứng dụng)

#### Chưa triển khai

- Progressive Delivery
- Canary Deployment
- Blue/Green Deployment
- Rollback Automation
- Environment Promotion

### Enterprise thường bổ sung

#### Deployment Strategy

- Progressive Delivery: Triển khai từng bước và theo dõi hệ thống.
- Canary Deployment: Chỉ một phần người dùng sử dụng phiên bản mới.
- Blue/Green Deployment: Hai môi trường chạy song song để chuyển đổi nhanh.
- Rollback Automation: Tự động quay về phiên bản trước nếu phát hiện lỗi.

#### Release Management

- Environment Promotion: Promote Image từ Dev → Staging → Production.

---

## 9. Platform Automation

### Tự động hóa việc quản lý Platform

#### Đã triển khai

- GitOps Bootstrap (Khởi tạo Platform từ Git)
- Helm Packaging (Đóng gói và tái sử dụng ứng dụng)

#### Chưa triển khai

- Terraform
- Ansible
- Automated Cluster Bootstrap
- Automated VM Provisioning
- Self-service Platform

### Enterprise thường bổ sung

#### Infrastructure as Code

- Terraform: Quản lý VM, Network và Cloud Resource bằng code.
- Ansible: Tự động cấu hình Server và cài đặt phần mềm.

#### Automation

- Automated Cluster Bootstrap: Tự động tạo Cluster và cài đặt Platform.
- Automated Provisioning: Tự động tạo hạ tầng mới.

#### Developer Platform

- Self-service Platform: Cho phép Developer tự tạo Namespace, Database hoặc Deploy Application mà không cần Platform Team.

---

## 10. Reliability

### Đảm bảo Platform hoạt động ổn định

#### Đã triển khai

- HA K3s Cluster (3 Control Plane)
- Longhorn Replication
- Backup lên MinIO

#### Chưa triển khai

- Restore Test
- Disaster Recovery Test
- Chaos Engineering

### Enterprise thường bổ sung

#### Disaster Recovery

- Automated Restore: Tự động khôi phục khi gặp sự cố.
- DR Testing: Kiểm tra định kỳ khả năng khôi phục hệ thống.

#### High Availability

- Multi-cluster: Vận hành nhiều Cluster để dự phòng.
- SLA / SLO: Đo lường độ sẵn sàng của Platform.

#### Resilience

- Chaos Engineering: Chủ động tạo lỗi để kiểm tra khả năng chịu lỗi.

---

## 11. Compliance & Governance (bất khả thi cho homelab nếu muốn chuẩn)

### Chưa triển khai

- Kubernetes Audit Log
- Vault Audit Log
- Policy Engine (Kyverno / OPA)
- Image Policy
- Compliance Scan

### Enterprise thường bổ sung

#### Audit

- Audit Log: Ghi lại toàn bộ thay đổi trên Platform.

#### Policy

- Policy Engine: Kiểm tra tài nguyên trước khi cho phép Deploy.
- Image Policy: Chỉ cho phép Image từ Registry tin cậy.
- Required Labels: Bắt buộc Resource có Label theo quy định.

#### Compliance

- CIS Kubernetes Benchmark
- ISO 27001
- SOC 2
- PCI DSS
- HIPAA
- GDPR

---

# Tổng kết Tech Stack

## Infrastructure

- Proxmox VE
- OPNsense
- K3s
- MinIO
- Docker Registry

## GitOps

- ArgoCD
- Helm

## Networking

- Traefik
- Gateway API
- HAProxy
- Unbound DNS
- Wireguard VPN

## Storage

- Longhorn
- MinIO

## Security

- Vault
- External Secrets

## Observability

- Prometheus
- Grafana
- Loki
- Tempo
- OpenTelemetry

---

# Homelab Infrastructure

## Virtualization

- Proxmox VE 9.1.9
- 8 vCPU
- 24 GB RAM
- 1 TB SSD
- 1 TB HDD

## Virtual Machines

| VM | Chức năng | Cấu hình |
|-----|-----------|----------|
| OPNsense | Firewall, Router, DNS, Reverse Proxy, VPN | 2 vCPU, 2 GB RAM |
| Bastion | SSH Gateway | 2 vCPU, 512 MB RAM |
| k3s Server | Control Plane | 2 vCPU, 6 GB RAM |
| k3s Worker 1 | Worker Node | 2 vCPU, 4 GB RAM |
| k3s Worker 2 | Worker Node | 2 vCPU, 4 GB RAM |
| MinIO | Object Storage | 1 vCPU, 1 GB RAM |
| Docker Registry | Private Container Registry | 1 vCPU, 512 MB RAM |

## Kubernetes

- k3s v1.35.5+k3s1
- 1 Control Plane
- 2 Worker Nodes

## Platform Components

- ArgoCD
- Traefik
- Gateway API
- Longhorn
- Vault
- External Secrets
- Prometheus
- Grafana
- Loki
- Tempo
- OpenTelemetry

## Repository

https://github.com/hieu190400/gitops-homelab