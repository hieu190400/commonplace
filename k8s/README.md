## Nói sơ qua

Mình học K8s là do nhiều công ty giờ đòi K8s nhiều quá. Trước đó chủ yêu làm AWS Cloud dự án nhỏ và vừa nên không chọn k8s được. 

Cấu hình và techstack sơ bộ 
- `Proxmox 9.1.9`: 8vCPU + 24GB RAM + 1T SSD + 1T HDD
- 1 VM `Opnsense 26.1.6_2-amd64` làm router: 2vCPU + 2GB RAM
- 1 VM bastion để SSH: 2vCPU + 0.5GB RAM
- 1 VM `k3sServer v1.35.5+k3s1` làm control panel node: 2vCPU + 6GB RAM
- 2 VM `K3sworker v1.35.5+k3s1` làm worker node: mỗi thằng 2vCPU + 4GB RAM

Repo: https://github.com/hieu190400/gitops-homelab

## 1-7-2026

Giờ mà đi học hết tụi controller chắc mấy tháng chưa xong. Học cái gì để debug khi có lỗi thôi. Quậy để lỗi rồi học.