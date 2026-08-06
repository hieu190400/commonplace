## Định nghĩa ban đầu

Lên mạng tìm thử coi có 1 đống cách định nghĩa Devops. Bóc thử 1 cái:

> DevOps là sự kết hợp giữa Development và Operations nhằm tăng tốc độ phát triển phần mềm thông qua văn hóa, quy trình và tự động hóa.

Hiểu gì chết liền, 1 đống câu hỏi phát sinh:
- Rồi nó không phải là 1 vị trí như frontend, backend à? 
- Rồi có người nói nó là 1 tư tưởng? Ủa rồi tuyển người làm công tác tư tưởng à?
- Có người nói devops là CI/CD? rồi sao? chỉ làm CI/CD?
- Còn nữa mà làm biếng ghi,...

Thêm cái nữa là mỗi công ty định nghĩa mỗi khác nữa, lú hết cả đầu.

## Quá trình tìm hiểu

Sau khi bay tùm lum trên google, youtube rồi qua chatgpt. Hỏi `devops là gì` rồi nhận lại đủ thứ định nghĩa. Tôi thấy nên đi theo hướng tìm hiểu mục đích: `tại sao lại đẻ ra devops, trước đâu có`.

Giả sử công ty chỉ có đúng 1 thằng dev

1 mình nó lo hết.

...

Rồi lên 5 người.

Có chuyện gì bắt đầu xảy ra?

...

20 người?

100 người?

## Đầu tiên, 1 công ty phần mềm đang làm cái gì?

```
Khách hàng có nhu cầu
        │
        ▼
 Product / Business
        │
        ▼
     Developer
        │
        ▼
    Build & Test
        │
        ▼
     Release
        │
        ▼
    Production
        │
        ▼
     User dùng
```

Nói đơn giản là:
- Khách muốn cái phần mềm.
- Công ty lên kế hoạch.
- Thực hiện việc code.
- Tìm cái chổ nào nào chạy cái ứng dụng đó để phục vụ khách.

## Tiếp theo, quá trình đi tới phần mềm hoàn chỉnh.

Bỏ qua mấy cái bước lên kế hoạch này nọ đi. 1 quy trình thường sẽ là như vầy.

0. Tìm chổ nào để code chạy phục vụ khách hàng trước. (Gọi là `nơi A`)
2. Code xong 
3. Review code 
4. Test code
5. Build code
6. Deploy code.

Cơ bản là như vậy, nhưng giờ bắt đầu tới phần vui nè.

## Phát triển rộng ra, mô hình trách nhiệm.

### 1. Đơn giản nhất (All in one)

Nếu chỉ có 1 người(Gọi là Dev A) thì người đó sẽ làm như sau:

1. Code xong tự test luôn.
2. Truy cập lên `nơi A` để cài môi trường/build/chạy code.
3. Dev kiêm luôn việc quản lý `nơi A`. Có gì là chỉ có Dev đụng được. 

Nhìn đơn giản chưa? miễn nó chạy là được.

### 2. Rồi kiến trúc phần mềm sẽ như thế nào? (Infra, Network)

Dev A nói: "Kệ nó, chạy được là được,deploy lên 1 máy mở port public hết là xong". 

0. Dev A dựng kiến trúc hệ thống.

### 3. Thêm Tester

Dev A nói em làm nhiều việc quá mệt. Công ty tuyển thêm người. Lúc này Dev sẽ làm:

1. Code (Dev làm)
    - Test sẽ có nhân sự mới

### 4. Dev than tiếp

Dev A than: đang ngồi code tự nhiên khách báo lỗi, em phải lên `nơi A` để điều tra và sửa nên chưa code xong => công ty lại tuyển thêm 1 ông.

1. Ông mới vào thay chổ Dev A kia.
2. Có gì muốn deploy thì gọi Dev A.
3. Dev A sẽ ngồi coi `nơi A` luôn.

### 5. Deploy nhiều

Dev A đang ngồi làm thì bị gọi vào phòng họp, có 1 dự án mới anh muốn em tham gia. Bùm Dev A được giao cho công việc 2 và 3 của dự án mới.

2. Trong 2 dự án muốn deploy thì cứ gọi Dev A
3. Dev A sẽ coi luôn cho 2 dự án.

### 6. Dev A sắp mặt khi số lượng dự án tăng lên. (CD)

Dev A 1 ngày đẹp trời được giao thêm 2,3 dự án cho phần 3 và 4. Từ sáng tới tối chỉ ngồi deploy... Mấy Dev nói ông này giờ làm chậm rì à.

=> Ông Dev A nghĩ ra sao không tự động hóa, mấy cái này giống nhau mà ta.

2. Dev A dùng 1 cái gì để tự động deploy và tạo 1 cái nút cho mấy dự án. Chỉ cần bấm nút là xong. Dev giờ rãnh.

### 7. Tự động khắp nơi (CI)

Dev nghĩ: "deploy tự động được kìa, test thì sao?". Dev A sau đó bắt tester phải viết 1 phần mềm có thể chạy để test code. Dev A gọi phần mềm đó trước khi nhấn nút deploy. 

1. Code (Dev làm)
    - Test viết code để test.
2. Trước khi deploy thì bấm nút để chạy test trước.

### 8. Monitoring (xem log)

Dev A nghĩ: "Bây giờ mình chỉ còn chờ dự án mới và tự động nó.". Giờ mình ngồi coi `nơi A` cả ngày, nhàn vl.

Nếu có lỗi thì vô coi log là xong. Lười quá thì chọi nguyên cái lỗi đó cho team dev. Oh nó không có log lỗi nè, dev sửa đi chứ.

Một thời gian nhàn nhã của `Dev A`

### 9. Số lượng người dùng tăng lên. (metric)

Dự án thành công, có vài dự án lượng người dùng tăng lên. Tự nhiên hệ thống chậm bất thường. Dev A ngồi khoanh chân: "Chà hệ thống đang hoạt động kìa, bấm điện thoại thôi". 

Đột nhiên, báo cáo tới, bộ phận phục vụ khách hàng nhận than phiền hệ thống chậm quá, hồi bữa nó chết mấy tiếng luôn làm tôi không làm được gì. 

Dev A bị sếp la. Ngay lặp tức Dev A ngồi tìm cách. Dev A tìm thấy cái gọi là metric. Nhìn vô thấy số CPU cao bất thường, vậy là server này yếu rồi, nâng lên đi.

Dev tiếp tục tra, sau 1 thời gian thấy RAM nhảy lên cao rồi tụt xuống rất thấp (OOM). Dev A lôi dev ra chửi code sao bị ăn Ram rồi kìa.

3. Thế là Dev A phải ngồi coi metric mỗi ngày.

### 10. Hệ thống cảnh báo. (Alert)

Dev A ngồi coi vài ngày thì thấy chán, có cái màn hình hiện chỉ số thì có gì coi. Dev A suy nghĩ hay mình tạo 1 cái cảnh báo đi, nếu CPU> 80 thì báo tới điện thoại. => Thế là hệ thống cảnh báo ra đời.

`Dev A` tiếp tục nhàn.

3. Dev A dùng cảnh báo cho metric và log

### 11. Công ty ổn định, bên trên muốn biết tiền bỏ ra để thuê `nơi A` (Bill)

Dev lúc này nói: "Hả. À thôi để coi bill, ... Ủa sao mình làm cái này luôn ta? À thôi kệ". 

3. Dev A kiêm luôn báo cáo bill.

### 12. Số lượng khách hàng tăng lên nữa. (Scale)

Dev A đang ngồi bình thường tự nhiên Alert báo tùm lum. Tự nhiên khách hàng đổ vào xài nhiều vậy? Mở thêm máy thôi. Thêm cái load balance nữa nè.

=> Máy mở nhưng không tắt đâu nên bill cuối tháng lên cao.

### 13. Chuyển đổi `Nơi A` sang `Nơi B`. (Cloud)

Dev đang ngồi thì nghe tin, sếp nói Bill giờ cao quá, máy giờ xài không tối ưu, sao nhiều máy không ai sài quá? sao không tắt bớt đi?

Dev A ngó qua thấy `Nơi B` có tính năng Auto scale gì đó khá hay. Dev A đề xuất chuyển qua `Nơi B`.

Hay quá, lãnh đạo duyệt. Xong Dev A nhận chỉ thị là mình làm luôn...

3. Dev A thực hiện migrate luôn. Xây lại hết.

### 14. Một hôm, có khách hàng lớn đòi xem database. (permission)

Dev A nhìn và nói: "Coi làm khỉ gì vậy? Ok để coi, hình như có cái gọi là phần quyền. Ủa mà mình làm luôn à?".


### 15. Số lượng dự án, môi trường phìn to (task allocation).

Dev A migrate xong thì quay lại nhìn tổng thể. ??? !!! Đệt giờ mình nắm hơn 30 cái dự án rồi à? Hình như cái đó làm cái kia đúng ko?

Rồi hình như khách hàng kêu phân quyền gì nữa, rồi cách ly tùm lum gì đó.

Ê công ty mướn thêm người đi, 1 mình tui sao làm nổi cái đống này hả?

## Phase 2, khi hệ thống đã đủ phức tạp.

Câu truyện kia đủ dài rồi, đơn giản là như vầy

```
DevOps quá tải → tách Platform.
Production quá quan trọng → tách SRE.
Cloud quá lớn → tách Cloud Engineer.
Security quá phức tạp → tách Security Engineer.
Database quá lớn → tách DBA.
Chi phí cloud quá cao → tách FinOps.
```

Khi làm devops mỗi công ty sẽ có 1 số trách nhiệm riêng, tùy công ty tách nhưng nó sẽ có thể bao gồm.

| Trách nhiệm            | Mục tiêu                        | Sau này có thể tách thành |
| ---------------------- | ------------------------------- | ------------------------- |
| Build & Deploy         | Đưa code lên production an toàn | Platform                  |
| CI/CD                  | Tự động hóa pipeline            | Platform                  |
| Infrastructure         | VM, Kubernetes, Cloud           | Cloud/Infra Engineer      |
| Infrastructure as Code | Quản lý hạ tầng bằng code       | Platform                  |
| Monitoring             | Thu metric, log, trace          | Observability             |
| Alerting               | Phát hiện sự cố                 | SRE                       |
| Incident support       | Điều tra khi production lỗi     | SRE                       |
| Capacity               | Scale, autoscaling              | SRE                       |
| Secrets                | Quản lý secret, certificate     | Security                  |
| Networking             | DNS, LB, Firewall, VPC          | Network Engineer          |
| Backup & DR            | Backup, khôi phục               | Infra                     |
| Cost                   | Theo dõi chi phí cloud          | FinOps                    |
| Developer tooling      | Giúp dev làm việc nhanh hơn     | Platform                  |


## Kết luận

Làm được 1 lúc thì lại lười... Tóm lại 1 cái cho xong luôn.

Sau khi tìm hiểu thì. 

Trong 1 công ty phần mềm, có đúng 2 loại công việc thôi:

1. Làm ra sản phẩm.
    - Lên kế hoạch
    - Thiết kế
    - Code
    - Test
    - QA...
2. Giữ cho sản phẩm chạy.
    - Deploy
    - Server
    - Network
    - Database
    - Monitoring
    - Alert
    - Backup
    - Security
    - Cost
    - Scale

=> Có chỗ gọi là Sysadmin|Devops|Infrastructure,... Tùm lum hết.

Rồi sao đó:

- Deploy nhiều quá -> tách thành team platform
- Monitoring nhiều quá -> tách thành SRE
- Security tùm lum -> mướn team security

```
├── Deploy -------- Platform
├── Server -------- Infra
├── Monitoring ---- SRE
├── Security ------ Security
├── Database ------ DBA
├── Cost ---------- FinOps
```

Rồi devops đâu mất tiêu rồi? Thấy hay chưa?

=> Kết luận lại DevOps chỉ là người đang ôm những trách nhiệm đó ở thời điểm hiện tại. Nếu sau này nó lớn hơn, devops sẽ bị tách ra tùm lum thành các team khác nhau.

```
Nếu vẫn chưa hiểu DevOps làm gì thì nhìn đơn giản thế này.

Đứa nào bị chửi khi hệ thống có vấn đề, thì đó là việc của nó.

- Code lỗi? → Dev.
- Test thiếu? → QA.
- Deploy lỗi? → DevOps.
- Server chết? → DevOps.
- CPU 100%? → DevOps.
- Database đầy? → DevOps.
- Cloud bill tăng? → DevOps.

Rồi một ngày công ty đủ lớn.

- CPU 100%? → SRE.
- Cloud bill? → FinOps.
- IAM? → Security.
- Database? → DBA.
- Deploy? → Platform.

Đứa bị chửi không còn là DevOps nữa.
Mỗi team sẽ bị chửi một phần.
```