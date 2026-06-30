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
    - Bên trong cluster có 2 loại máy hay gọi là node. control panel node hoặc worker node.
        - control panel là node điều kiển. tụi controller hay nằm ở đó. 
        - worker là node chạy workload nói nôm na là tụi làm công đưa gì chạy đó. 
    - Mỗi chương trình chạy trong máy đó gọi là pod, trong pod có 1 hoặc nhiều container.

4. (trả lời luôn cho 3.1 và 3.2)
    - K8s hoạt động theo kiểu khai báo mong muốn. Cứ nói tôi muốn 3 cái nginx và k8s sẽ tìm cách.
    - Đầu tiên, k8s dĩ nhiên sẽ có bộ controller tiêu chuẩn của nó. Và controller hay app gì đều chạy trên pod hết, controller cũng là 1 app thôi.
    - Thứ 2: kubelet là thằng đặt biệt, nó chạy trên OS chứ không phải trong k8s. 
    - Thứ 3: k8s sẽ có bộ khởi tạo (bootstrap) gồm các controller thiết yếu trước trên control panel node. dĩ nhiên, kubelet là thằng làm việc khởi tạo. Khi controller khác chết thì sẽ có tụi controller này cứu lại.

5. Gần như toàn bộ đều là vậy.
   