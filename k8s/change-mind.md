# 1-7-2026

Kubernetes gồm

- Một cái database (etcd)
- Một API để đọc/ghi cái database (API Server)
- Rất nhiều app cùng đọc database đó và làm việc tùy theo logic của app.

Học 1 Chức năng chỉ cần 
- App Nó ngó object nào trong etcd hay trong runtime?
- App Ngó xong thì nó chạy gì?

Ngộ ra chân lý: Mình toàn gọi API server để CRUD cái etcd đúng nghĩa đen. Đúng là lương có cao mấy cuối cùng cũng chỉ CRUD :)

# 30-6-2026
K8s là 1 đống controller chạy song song, nó sẽ để ý đến 1 số resource nhất định lưu trong etcd.

Học 1 Chức năng chỉ cần 
- coi nó lưu resource gì? 
- Nó có controller không?
- Nó có chạy pod không?

là xong.

