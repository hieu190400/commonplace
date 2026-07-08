### Định nghĩa Enterprise
Đi làm hay đi học người ta thường nói là chuẩn phần mềm Enterprise. Giờ bắt đầu tìm hiểu thì nó là vầy:
 | Tiêu chí          | Ý nghĩa                                         |
| ----------------- | ----------------------------------------------- |
| Scalability       | Chịu được hàng nghìn hoặc hàng triệu người dùng |
| Reliability       | Hoạt động ổn định                               |
| Security          | Bảo mật cao                                     |
| Availability      | Hạn chế downtime                                |
| Monitoring        | Có giám sát hệ thống                            |
| Logging           | Ghi log đầy đủ                                  |
| Audit             | Theo dõi mọi thay đổi                           |
| Backup            | Có cơ chế sao lưu                               |
| Disaster Recovery | Khôi phục sau sự cố                             |
| RBAC              | Phân quyền theo vai trò                         |
| SSO               | Đăng nhập một lần                               |
| API               | Dễ tích hợp với hệ thống khác                   |
| Compliance        | Đáp ứng các yêu cầu tuân thủ nếu cần            |

Nhìn là thấy tốn tiền rồi. Công ty nhỏ hay vừa đú theo chắc sạc nghiệp. 

Thành ra mấy cái tools dạng đó đa phần toàn siêu nặng và phức tạp. Ngoại trừ vài thằng như nginx này nọ.

Mà với vai trò dev tôi thích kiểu nhanh gọn hơn.

### Mấy cái vendor lock-in này nọ.

Đi làm thì tôi thấy được 4 vụ. 
1. MongoDB chơi license mới, thế là tụi cloud tự đẻ ra phiên bản của mình
2. Elasticsearch với opensearch. Khi bạn đủ nổi tiếng thì bạn sẽ phải kiếm tiền. mấy đứa khác đâu bỏ bạn được, tích hợp sâu vô hệ thống rồi. 
3. Serverless framework lúc đầu v1,2,3 thì hỗ trợ khá rộng đủ cloud. Lúc sau lên v4 tuyên bố, tao thu tiền nha. Thêm 1 cái nữa là giờ tập trung hỗ trợ AWS. Cái bug tui đợi nó fix không thèm fix luôn, kêu up lên v4 đi.
4. Redis OSS đổi license. Vụ này thì không biết nói sao, cũng không ảnh hưởng mình nhiều lắm. Chủ yếu là tụi cloud dính. Thế là nó đẻ ra Valkey trong nháy mắt. quảng cáo free forever khịa redis.

### Rồi giờ mới biết CNCF nè.

Viết tắt của Cloud Native Computing Foundation. Vấn đề là thằng nào cũng sợ vendor lock-in hết. Thành ra Google khởi sướng CNCF với kubernetes được feed vào đầu tiên.

Ý tưởng nói thô ra là cho The Linux Foundation vận hành, mấy ông lớn feed dự án vào đó. Cả đám sẽ đứng ra maintain và phát triển dự án.

Lợi ích là không sợ vendor lock-in và dự án cũng được nhiều bên hợp tác phát triển hơn. Đở bị cạnh tranh hơn.

Sau này nhiều bên nhảy vào tham gia và nhiều dự án được feed vào, giờ nó thành chuẩn cho doanh nghiệp xem xét.