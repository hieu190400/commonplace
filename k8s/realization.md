# 30-6-2026

1. K8s là một cục API siêu bự và có khả năng thêm feature siêu dễ để mở rộng.
2. K8s gồm 
    - Cục API bự chà bá.
    - etcd: Database lưu trạng thái của RD resource và này nọ chưa tìm hiểu.
    - RD(resource definition): chỉ là mấy cái object dạy API biết có class/ resource nào.
    - Controller: là mấy quản lý đăng ký với API là tôi sẽ quản lý resource nào đó. Khi nào nó thay đổi hãy cho tôi biết để hành động.
    - kubelet: là agent trên worker để kết nối worker với control panel.
    - kubectl: là command line để tương tác với API.

3. Kiến trúc tổng thể gồm
    - Cluster là đơn vị bự nhất đầy đủ chức năng. Dĩ nhiên nếu cloud này nọ thì người ta đặt nhiều cluster để dự phòng này nọ.
    - Bên trong cluster có 2 loại máy hay gọi là node(1 máy vật lý hoặc máy ảo). control panel node hoặc worker node.
        - control panel là node điều kiển. tụi controller hay nằm ở đó. 
        - worker là node chạy workload nói nôm na là tụi làm công đưa gì chạy đó. 
    - Mỗi chương trình chạy trong máy đó gọi là pod, trong pod có 1 hoặc nhiều container.

4. (trả lời luôn cho 3.1 và 3.2)
    - K8s hoạt động theo kiểu khai báo mong muốn. Cứ nói tôi muốn 3 cái nginx và k8s sẽ tìm cách.
    - Đầu tiên, k8s dĩ nhiên sẽ có bộ controller tiêu chuẩn của nó. Và controller hay app gì đều chạy trên pod hết, controller cũng là 1 app thôi.
    - Thứ 2: kubelet là thằng đặt biệt, nó chạy trên OS chứ không phải trong k8s. 
    - Thứ 3: k8s sẽ có bộ khởi tạo (bootstrap) gồm các controller thiết yếu trước trên control panel node. kubelet là thằng làm việc khởi tạo. Khi controller khác chết thì sẽ có tụi controller này cứu lại.

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