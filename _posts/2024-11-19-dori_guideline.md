---
layout: post
title: DRAFT_Hướng dẫn sử dụng Dori để nhận dạng các Key Information
---

Trong bài viết này, Mình sẽ hướng dẫn bạn cách sử dụng Dori để nhận dạng và trích xuất các thông tin chính. Ví dụ, nếu bạn cần nhận dạng các thông tin như **Họ Tên** và **ID** trên căn cước công dân, thì **Họ Tên** và **ID** là những thông tin cần được trích xuất. Để thực hiện điều này, bạn sẽ cần trải qua các bước sau:  
1. Thu Thập dữ liệu: mình giả sự bạn đã có dữ liệu muốn nhận dạng thông tin, nếu bạn không có dữ liệu vui lòng quay lại sau
2. Đánh nhãn dữ liêu: là quá trình dùng con người để tạo ra các thông tin cần thiết mà bạn cần model học theo. Để rút trích thông tin chính, bạn cần đánh các loại nhãn sau:
- Phát hiện văn bản  
- Nhận dạng văn bản  
- Xác định thứ tự đọc  
- Trích xuất thông tin chính  
3. Huấn luyện mô hình: là quá trình dạy mô hình máy học trên tập dữ liệu đã được gán nhãn ở bước trên
4. Kiểm tra kết quả nhận dạng: bước này đơn giản là bạn kiểm tra mô hình đã tốt hơn để sử dụng hoặc đánh nhãn tiếp và huấn luyện lại.
  
Các bước trên có thể được thực hiện dễ dàng trên Dori, nơi cung cấp các chức năng sẵn có như đánh nhãn, huấn luyện, triển khai và kiểm tra mô hình. Dori còn tăng tốc quá trình đánh nhãn và huấn luyện bằng các mô hình pretrained sẵn cho tiếng Việt và tiếng Anh.

## Tạo Dự Án Mới  
Đầu tiên, hãy tạo một dự án mới trên Dori bằng cách nhấn vào **Create Project**, sau đó điền tên dự án, mô tả và phân loại. Hãy chọn hai công cụ cơ bản là **text detection** và **key information extraction**. Tiếp theo, nhấn vào **Upload Image** để chọn các ảnh cần đánh nhãn.  

**Gợi ý**: Bạn có thể bắt đầu với 20 ảnh cho dự án thử nghiệm nhỏ và từ 100-300 ảnh để huấn luyện mô hình phục vụ thực tế. Không cần tải lên tất cả ảnh ngay từ đầu, bạn có thể bổ sung thêm trong phần **Setting** của dự án sau này. Sau khi tải lên ảnh, hệ thống sẽ tự động chạy mô hình phát hiện văn bản mặc định để giúp giảm thời gian đánh nhãn.

<img width="1424" alt="Screenshot 2024-11-19 at 15 20 06" src="https://github.com/user-attachments/assets/f3e2d424-9cf0-46e1-9580-6d4a2010c85d">

Nhấn **Done** để hoàn tất bước tạo dự án. Sau đó, bạn nhấn vào **Label** để bắt đầu đánh nhãn các ảnh vừa tải lên.

## Đánh Nhãn  
Đánh nhãn là quá trình tạo dữ liệu mẫu để huấn luyện mô hình.  

### 1. Phát hiện và Nhận dạng Văn bản (Text Detection & Recognition)  
Trước khi hướng dẫn sử dụng Dori, chúng tôi sẽ mô tả quy trình 3 bước trong việc phát hiện văn bản để giúp bạn hiểu mục đích của từng bước.

**Phát hiện văn bản là gì? (Text Detection)**  
Đây là bước đầu tiên trong xử lý tài liệu. Phát hiện văn bản giúp xác định các vùng có chứa văn bản trong ảnh hoặc tài liệu bằng cách tạo các hộp giới hạn (bounding boxes) xung quanh các đoạn, dòng hoặc cụm từ.

**Nhận dạng văn bản là gì? (Text Recognition)**  
Sau khi xác định được vùng chứa văn bản, bước tiếp theo là nhận dạng nội dung trong các vùng đó, chuyển đổi hình ảnh chữ thành văn bản số mà máy tính có thể xử lý, thường sử dụng các mô hình OCR (Optical Character Recognition).

**Xác định thứ tự đọc là gì? (Reading Order Detection)**  
Trong các tài liệu phức tạp, thứ tự đọc không luôn rõ ràng, đặc biệt khi có nhiều cột, bảng hoặc bố cục đặc biệt. Xác định thứ tự đọc giúp đảm bảo xử lý đúng thông tin, hỗ trợ việc trích xuất dữ liệu chính xác.

Dori tích hợp ba bước này trong một công cụ gọi là **Text Detection**. Công cụ này hiển thị các từ cùng với vị trí hộp giới hạn của chúng. Dori sẽ xử lý văn bản của bạn theo từng từ, do đó, bạn cần chỉnh sửa, xóa hoặc thêm các hộp và văn bản có sẵn để đảm bảo tính chính xác, cũng như sắp xếp lại thứ tự đọc nếu cần. Dori cung cấp hai loại hộp cơ bản: **rectangle** và **polygon**. Với các văn bản thông thường, chỉ cần chọn **rectangle** là đủ, còn các chữ phức tạp mới cần dùng **polygon**.

Khi vẽ hộp xong, bạn sẽ được yêu cầu nhập văn bản, nội dung này sẽ hiển thị trong phần **document** bên trái và **reading order** bên phải để tiện theo dõi.
### Làm như nào để đánh nhãn ở bước này chính xác?
Khi vẽ bounrady box cần phải vẽ cho từng từ, không phải vẽ cho từng kí tự, cũng ko phải vẽ cho từng câu hay đoạn. Boundary box cần bao phủ chính xác từng từ, ko đươc thiếu dấu câu hay nét, không được overlap với những boundary box khác. Các bạn xem thêm minh hoạ phía dưới

### 4. Trích xuất Thông tin Chính (Key Information Extraction)  
**Trích xuất thông tin chính là gì?**  
Sau khi văn bản được nhận dạng và sắp xếp, bạn cần xác định thông tin chính cần trích xuất. Ví dụ, nếu bạn cần rút trích **Họ Tên** và **ID**, hãy gắn nhãn cho hai loại thông tin này và chọn cụm từ tương ứng trong văn bản. Bạn có thể tham khảo video dưới để biết thêm chi tiết. 


#### Làm như nào để đánh nhãn ở bước này chính xác?
Khi chọn các thông tin cần nhận dạng, các bạn phải select toàn bộ đối tượng đó trong văn bản. Ví dụ, bạn muốn đánh nhãn `Nguyễn Văn A` là tên thì phải bôi đen liên tục cụm từ này, không được select Nguyễn riêng, Văn riêng, A riêng. Nếu Nguyễn Văn A không nằm liên tục nhau trên văn bản, bạn cần quay lại bước text detection để thay đổi thứ tự đọc

## Sử dụng tính năng self train để tăng tốc quá trình đánh nhãn
Quá trình đánh nhãn thường mất nhiều thời gian và công sức, nhưng Dori cung cấp tính năng **Self Train** để giúp bạn tận dụng mô hình được huấn luyện trên dữ liệu đã đánh nhãn để tự động áp dụng vào dữ liệu chưa đánh nhãn. Thông thường, bạn chỉ cần đánh nhãn 10-20 mẫu, sau đó huấn luyện mô hình trên dữ liệu này. Khi huấn luyện xong, bạn chỉ cần nhấn **Self Train** để áp dụng mô hình lên phần dữ liệu còn lại.

## Huấn luyện Mô Hình  
Huấn luyện mô hình là quá trình sử dụng tập dữ liệu đã được đánh nhãn để xây dựng mô hình nhận dạng. Trên Dori, bạn có thể huấn luyện mô hình trên tập dữ liệu của mình bất cứ khi nào cần. Việc huấn luyện có thể được thực hiện sau mỗi 10, 50, 100, hoặc 1000 mẫu dữ liệu để theo dõi độ chính xác của mô hình, sau khi cập nhật hoặc bổ sung dữ liệu, hoặc khi muốn nhận dạng thêm trường thông tin mới.

Để bắt đầu huấn luyện trên Dori, bạn cần vào phần **Setting**, chọn **Train**, sau đó chọn mô hình phù hợp và cấu hình các tham số huấn luyện. Nhấn **Add** để khởi động quá trình huấn luyện mô hình. Hệ thống sẽ gửi email thông báo khi quá trình huấn luyện bắt đầu và kết thúc. Bạn có thể theo dõi lịch sử và độ chính xác của mô hình trong phần log.

### Huấn luyện Mô Hình để rút trích thông tin chính. 
Để huấn luyện mô hình rút trích thông tin chính, các bạn cần phải huấn luyện trước các mô hình sau **text detection**, **text recognition**, **reading order detection** ( nếu cần), và sau đó mới huấn luyện mô hình cuối cùng là **key information extraction**. Điều quan trọng là 4 mô hình này cần phải huấn luyện tuần tự, tức là mô hình **text detection** cần huấn luyện xong trước rồi mới tới mô hình **text recognition**, sau khi mô hình này xong thì mới huấn luyện tuần tự 2 mô hình còn lại. Thứ tự huấn luyện là qua trọng và chỉ bắt đầu huấn luyện mô hình tiếp theo nếu mô hình trước đó đã chạy xong. 

Để hiểu rõ hơn, vui lòng tham khảo video dưới đây.


## Kiểm Tra Mô Hình  
Sau khi mô hình được huấn luyện xong, hệ thống sẽ tự động triển khai (deploy) mô hình để cho phép bạn kiểm tra và sử dụng. Hãy chuyển sang tab **API**, chọn mô hình tương ứng, tải lên hình ảnh và nhấn **Done**. Kết quả sẽ hiển thị ở bên phải để bạn tham khảo. Đồng thời, bạn cũng có thể sử dụng lệnh **curl** để kiểm tra mô hình. Để rõ hơn, vui lòng tham khảo video dưới đây.

