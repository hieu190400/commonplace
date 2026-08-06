# 6/8/2026

Lúc đầu cứ nghĩ: DevOps, SRE, Platform là mấy cái khác nhau. 

Cuối cùng gần như chỉ có `một đống trách nhiệm để giữ hệ thống chạy.` (Làm sao để code của dev lên Production nhanh nhất, an toàn nhất, và chạy ổn định nhất.)

Đống trách nhiệm này bao gồm:

- Infrastructure: Dựng server, cloud, network, container (K8s, Docker).
- CI/CD: Viết pipeline tự động build, test, deploy.
- Observability: Monitor, logging, alert khi hệ thống sập.
- Reliability & Incidents: Trực ca (On-call), fix sự cố, tối ưu uptime/SLA.
- Developer Experience (DX): Viết tool/script cho dev tự phục vụ (Self-service).

Tùy quy mô công ty mà người ta tách đống trách nhiệm đó thành nhiều team.

Nếu chưa tách thì thường gọi là DevOps.

Nếu tách hết rồi thì mất luôn DevOps.

Theo Gemini
```
[ Đống trách nhiệm giữ hệ thống chạy & an toàn ]
                       │
       ┌───────────────┼───────────────┐
       ▼               ▼               ▼
[ Reliability ]  [ Developer ]   [ Security & ]
  (Uptime/Ops)    (Platform)     (Compliance)
       │               │               │
       ▼               ▼               ▼
     SRE           Platform        SecOps /
                  Engineering       AppSec
```