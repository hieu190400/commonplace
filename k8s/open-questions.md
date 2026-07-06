# 6-7-2026
18. Cách để quản lý secret và xoay vòng trong k8s. Giờ đi tới vault. Hình như phải init? vậy init là gì?
19. Câu hỏi tốn siêu nhiều thời gian. Rồi làm sao cái k8s nó lấy được secret từ HashiCorp. 
20. Cái nhức đầu nè, auth là gì? làm sao để xác thực thằng external với nhà cung cấp. Hỗ trợ bao nhiêu cái?

# 4-7-2026
16. Giờ thử tới cái longhorn, nếu 1 rep thì lở node bị tắt sẽ ra sao? 
17. Rồi nếu 3 node, 2 rep, chết 1 node -> longhorn đẩy qua tạo rep trên node còn lại. Xong xuôi node đó lên lại, lúc này sao? dữ liệu vẫn còn trên A? dung lượng vẫn mất? 


---

# 3-7-2026
14. Cái volume này nọ là sao? lở pod chết thì sao? volume nó có tự chạy qua pod mới không?
15. Giờ tới cái longhorn, nó có lấy quá trớn không? giới hạn được không?


---

# 2-7-2026
12. Chắc là tìm hiểu thằng argocd trước đi.
13. Dính cái incident nên giờ phải coi cái server-side apply. Cách nó xử lý CRUD sao?

---

# 1-7-2026

7. Cái đống mặc định của k8s/k3s gồm những gì? có thể chia sao? 
8. Có cái nào cũ ít xài không? thay thế mới bằng cái nào? có cái khỉ nào stable cả chục năm như c/c++ mà không chuyển nổi qua rust không?
9. Khi học controller thì nên học cái gì? cái gì là cơ bản của controller để có thể xài cách thức đó qua nhiều controller?
10. Giờ tới cái network đi, chắc siêu nhức đầu. Cái luồng từ bên ngoài internet vô được cái container nó sao?
11. Giờ tới cái vụ kubectl, câu lệch sẽ có cú pháp sao? có chuẩn chung không?

---

# 30-6-2026

1. K8S là gì? 
2. K8S gồm cái gì?
3. Kiến trúc tổng thể từ bự tới nhỏ?
    
    3.1 Rồi tụi controller có khi nào nằm bên worker không? lở thằng controller chết thì tính sao? thằng nào hồi sinh nó lại?

    3.2 Rồi app chạy bằng gì? container? docker? hay chạy trên OS? Rồi tụi controller có phải app không?

4. Tổng quan của K8s?
5. Luồng khởi tạo K8S sao?
6. Giờ đụng tới cái controller. cái vụ khai báo mong muốn và k8s tự giải quyết.
