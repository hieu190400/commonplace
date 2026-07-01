# 1-7-2026

7. Chắc chia theo 
    - Compute: Pod, ReplicaSet, Deployment, StatefulSet, DaemonSet, Job, CronJob
    - Networking: ingress, Service,...
    - Storage: PersistentVolume
    - Configuration: Secret, ConfigMap,...
    - Security: RBAC, ServiceAccount,...
    - Scheduling: Scheduler
    - Observability: Metrics Server
    - Extensibility: Mấy cái gắn thêm vô.
    - Còn cả nùi nữa mà lười rồi, mốt tính.

8. Chatgpt nói có
    - ReplicationController gần như đã chết?? nghe hơi ...
    - Ingress dần bị gateway api thay.
    - Lúc trước tưởng docker mà giờ nó dùng containerd hay gì rồi
    - Endpoints và EndpointSlice.

9. Mới phát hiện có mấy thằng không có controller như Secret, ConfigMap... nên cái này chắc không xoáy controller nữa. Giờ chắc chia thành object với App.
    - Object nằm trong etcd, nó là chổ lưu thông tin, như cái database thôi. Mới coi thì nó là dạng key-value như NoSQL.
    - Tụi còn lại sẽ đọc Object này để làm việc, còn làm gì thì tùy từng thằng.
    - Mới sửa thành App vì chatgpt nói có mấy cái đâu phải controller như kubelet, API server. Mọi App trong K8s muốn đọc etcd đều phải đi qua thằng API server. 
    - Controller coi như là app có số lượng bự nhất đi, nó đơn giản là ngồi dòm 1 cái key trong etcd liên tục và cập nhật lại cho đúng với trạng thái đang chạy. Ví dụ đang chạy 1 nginx nhưng etcd nói 3 thì thằng controller thấy khác và sửa thành 3.
    - Sau khi vọc 1 hồi thì phát hiện ConfigMap không có controller. Vậy ai ngó nó? Thì ra nó chỉ đơn giản là data lưu trong etcd thôi, thằng nào cần thì đọc không thì thôi. 
    - Nói dài dòng khó hiểu thôi chốt là nó có etcd lưu data và mấy thằng khác thông qua API đọc data đó rồi làm việc.

10. Giờ đi từng khúc nè. lấy cái nginx của tui ra đi.
    - 1. Thằng nào public port ra đây?
        - ChatGPT trả lời siêu nhức đầu, cơ bản là có 1 thằng sẽ mở port trên Node và dẫn request vào trong.
    - 2. Vậy rồi lỡ có nhiều worker thì nó biết đi đâu?
        - Giờ chatGPT mới nói là có loadbalancer. Ok vậy coi như loadbalancer là thằng điều phối từ ngoài vào node đi.
    - 3. Giờ vào trong rồi phải có 1 thằng nào đó đẩy request đi chứ? nginx à? Rồi cái pod chạy nằm bên worker khác tính sao?
        - Thường người ta sẽ có cái Service đứng hứng. Service nó gôm được endpoint của app lại, là mấy cái ip với port á, giống target của loadbalancer. 
        - Rồi thằng kube-proxy sẽ đọc cái object EndpointSlice thằng Service lưu để đẩy request đi đúng vô pod.
    - 4. Tôi đang xài cái gateway API gì đó nó có thêm cái object định nghĩa loại route luôn như httproute,... để cho mấy thằng khác xài.
    - 5. Chatgpt mới nói là thằng kube-proxy nó lại không phải proxy. nó chỉ watch rồi sửa iptables trên máy thôi. Đúng chuẩn nhìn etcd rồi sửa. Nhức nhức cái đầu rồi, cái Service cũng chỉ là object và có thằng controller khác ngó nó mà làm việc. Vậy là mình đụng toàn bộ là mấy cái object trong etcd.

11. Có thể chia là kubectl \<action> \<resource> \<name> \<option>
    - action: get, edit, delete, apply, exec, describe,logs, patch, scale, rollout, top.
    - resource: toàn bộ trong etcd á. gọi cái nào cũng được
    - name: name của cái resource, à có cái events nữa.
    - option: chắc có -A, -o wide, -w, -f là tui xài nhiều.
---

# 30-6-2026

1. K8s là một cục API siêu bự và có khả năng thêm feature siêu dễ để mở rộng.
2. K8s gồm 
    - Cục API bự chà bá.
    - etcd: Database lưu trạng thái của RD resource và này nọ chưa tìm hiểu.
    - RD(resource definition): Định nghĩa resource cho K8s hiểu. Kiểu khai báo class/object á.
    - Controller: là mấy quản lý đăng ký với API là tôi sẽ quản lý resource nào đó. Khi nào nó thay đổi hãy cho tôi biết để hành động.
    - kubelet: là agent trên node để tụi máy thấy nhau.
    - kubectl: là command line để tương tác với API.

3. Kiến trúc tổng thể gồm
    - Cluster là đơn vị bự nhất đầy đủ chức năng. Dĩ nhiên nếu cloud này nọ thì người ta đặt nhiều cluster để dự phòng này nọ.
    - Bên trong cluster có 2 loại máy hay gọi là node(1 máy vật lý hoặc máy ảo). control panel node hoặc worker node.
        - control panel là node điều kiển. tụi controller hay nằm ở đó. 
        - worker là node chạy workload nói nôm na là tụi làm công đưa gì chạy đó. 
    - Pod cứ coi như 1 instance của app đi, để scale á, trong pod có 1 hoặc nhiều container.

4. (trả lời luôn cho 3.1 và 3.2)
    - K8s hoạt động theo kiểu khai báo mong muốn. Cứ nói tôi muốn 3 cái nginx và k8s sẽ tìm cách.
    - Đầu tiên, k8s dĩ nhiên sẽ có bộ controller tiêu chuẩn của nó. Và controller hay app gì đều chạy trên pod hết, controller cũng là 1 app thôi.
    - Thứ 2: kubelet là thằng đặt biệt, nó chạy trên OS chứ không phải trong k8s. 
    - Thứ 3: k8s sẽ có bộ khởi tạo (bootstrap) gồm các controller thiết yếu trước trên control panel node. kubelet là thằng làm việc khởi tạo. Khi controller khác chết thì sẽ có tụi này đảm nhiệm bật lại (cái Deployment hay kubelet cứu tùy loại).

5. Luồng khởi tạo.
    - Đầu tiên là kubelet agent sẽ bootstrap toàn bộ controller cơ bản của k8s lên control panel node. Sau khi xong rồi nó mới thành k8s hoàn chỉnh và nó hoàn toàn tuân theo quy trình khai báo mong muốn. 
    - Thằng đầu tiên lên là etcd, nó là thằng lưu object của cluster dạng key-value thôi. Kiểu Pod thì gồm thuộc tính nào á. Nó là databse không phải controller.
    - Thằng thứ 2 lên là API, nó là thằng duy nhất không phải controller. Nó là thằng giao tiếp trực tiếp với etcd. Mọi controller còn lại thì giao tiếp với nó.
    - Thằng thứ 3 lên là controller thiệt nè, nhưng nó là một bộ controller của K8s gồm Node, Deployment, Namespace,... 
    - Thằng thứ 4 lên là controller kube-scheduler nó chỉ làm 1 việc là chọn node cho pod thôi. Kiểu coi chổ nào trống là điền vô. 

6. Khai báo mong muốn    
    - Flow chung: Desired State (Object) -> API Server -> etcd -> Controller Watch -> Reconcile -> Actual State
    - Thằng controller nào cũng y chang vậy hết. Đầu tiên là khai báo object vô etcd thông qua thằng API rồi thằng controller cũng nói với API là nó muốn theo dõi cái object đó. 
    - Sau đó khi object đó đổi thì thằng API sẽ bắn thông báo tới thằng controller là object mày đăng ký đổi rồi đó và thằng controller sẽ biết và sửa lại cho đúng. Controller chỉ làm 1 việc là so sánh coi trạng thái hiện tại và mong muốn khác không.
    - Ví dụ: ReplicaSet Controller nó sẽ chỉ thông báo với API là: Ê tạo muốn coi cái object ReplicaSet và object Pod á, nó đổi thì nói. Sau đó nó đổi thì API sẽ bắn cho ReplicaSet Controller biết thay đổi và Controller tự sửa lại cho đúng.
    - Giống với tất cả các app khác gắn vô sau này như argocd, rồi log, rồi monitor. Mỗi thằng đều có controller của riêng mình. 
    - **Chốt lại chỉ là một đống controller chạy cùng lúc. Mỗi controller đều theo cùng một pattern (Watch → Reconcile). Cuối cùng thành ra học controller chứ không phải K8s, mỗi thằng chạy 1 kiểu**.

