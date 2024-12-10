---
layout: post
title: DRAFT_Hướng dẫn sử dụng DORI để nhận dạng các Key Information
---

Trong bài viết này, mình sẽ hướng dẫn bạn cách sử dụng DORI để nhận dạng và trích xuất các thông tin quan trọng từ tài liệu. Chẳng hạn, nếu bạn cần xác định Họ Tên và ID trên căn cước công dân, thì hai thông tin đó chính là mục tiêu mà bạn muốn mô hình nhận dạng. Về cơ bản, quy trình thực hiện gồm các bước chính sau:
1.	Thu thập dữ liệu: Giả sử bạn đã có sẵn tập dữ liệu đầy đủ. Nếu chưa, bạn cần tiến hành thu thập dữ liệu trước.
2.	Đánh nhãn dữ liệu: Đây là quá trình con người can thiệp để gắn nhãn các thông tin mà mô hình cần học. Để trích xuất thông tin chính, bạn cần đánh nhãn các thành phần:
 - Phát hiện văn bản
 - Nhận dạng văn bản
 - Xác định thứ tự đọc (không bắt buộc)
 - Trích xuất thông tin chính
3. Huấn luyện mô hình: Huấn luyện mô hình máy học trên tập dữ liệu đã được gán nhãn ở bước trên.
4. Kiểm tra kết quả nhận dạng: Đánh giá mô hình sau khi huấn luyện để xem kết quả đã đạt yêu cầu chưa. Nếu chưa, có thể thực hiện gán nhãn bổ sung và huấn luyện lại.

Thực hiện thủ công các bước trên đòi hỏi rất nhiều thời gian, chi phí, và công sức—từ chi phí đánh nhãn, quản lý dữ liệu, phát triển giải pháp, đến huấn luyện mô hình và cập nhật khi dữ liệu mới xuất hiện. Nếu bạn muốn nhận dạng hàng chục loại giấy tờ khác nhau, khối lượng công việc và thách thức sẽ gia tăng đáng kể.

Trong quá trình phát triển nhiều mô hình nhận dạng khác nhau, mình nhận thấy các bài toán này luôn tuân theo một quy trình chuẩn. Dựa trên kinh nghiệm đó, mình đã phát triển DORI, một nền tảng end-to-end chuyên về các giải pháp nhận dạng liên quan đến văn bản. DORI giúp rút ngắn thời gian xây dựng mô hình, đồng thời hỗ trợ bạn nhanh chóng tạo ra các giải pháp chất lượng cao cho cá nhân hoặc doanh nghiệp.

Hơn thế nữa, DORI không chỉ là một công cụ mà còn có thể được xem như một “baseline” để cộng đồng cùng tham khảo và nâng chuẩn chất lượng cho các bài toán nhận dạng tiếng Việt. Cụ thể, DORI cung cấp những tính năng sẵn có như:
- Giao diện đánh nhãn dữ liệu trực quan
- Quy trình huấn luyện tự động
- Công cụ triển khai và kiểm thử mô hình
- Mô hình pretrained cho tiếng Việt và tiếng Anh, giúp đẩy nhanh quá trình đánh nhãn và huấn luyện

Với DORI, bạn có thể tối ưu hoá quy trình xây dựng mô hình nhận dạng văn bản, giảm thiểu chi phí, công sức, và thời gian, đồng thời nâng cao chất lượng và tiêu chuẩn cho các giải pháp OCR trong ngôn ngữ tiếng Việt.

## Tạo Dự Án Mới  
Đầu tiên, hãy tạo một dự án mới trên DORI bằng cách nhấn vào **Create Project**, sau đó điền tên dự án, mô tả và phân loại. Hãy chọn hai công cụ cơ bản là **text detection** và **key information extraction**, ngoài 2 công cụ cơ bản ngoài DORI, còn cung cấp các công cụ khác như **Phân loại văn bản**, **Phân tích bố cục**, **Xác định cấu trúc bảng**, **Rút trích mối quan hệ**, v.v. Tiếp theo, nhấn vào **Upload Image** để chọn các ảnh cần đánh nhãn.  

**Gợi ý**: Bạn có thể bắt đầu với 20 ảnh cho dự án thử nghiệm nhỏ và từ 100-300 ảnh để huấn luyện mô hình phục vụ thực tế. Không cần tải lên tất cả ảnh ngay từ đầu, bạn có thể bổ sung thêm trong phần **Setting** của dự án sau này. Sau khi tải lên ảnh, hệ thống sẽ tự động chạy mô hình phát hiện văn bản mặc định để giúp giảm thời gian đánh nhãn.

<img width="1424" alt="Screenshot 2024-11-19 at 15 20 06" src="https://github.com/user-attachments/assets/f3e2d424-9cf0-46e1-9580-6d4a2010c85d">

Nhấn **Done** để hoàn tất bước tạo dự án. 

Sau đó, bạn nhấn vào **Label** để bắt đầu đánh nhãn các ảnh vừa tải lên, do bạn đã chọn 2 tool **text detection** và **key information extraction** nên trên giao diện đánh nhãn sẽ hiển thị 2 công cụ để giúp bạn đánh nhãn cho các bài toán này. Cụ thể về chức năng của 2 công cụ này sẽ mô tả chi tiết ở phần tiếp theo. 

## Đánh Nhãn  
Đánh nhãn là quá trình tạo dữ liệu mẫu để huấn luyện mô hình, tuỳ mỗi mô hình và bài toán khác nhau mà có cách đánh nhãn khác nhau. Đồng thời công cụ đánh nhãn chuyên biệt sẽ giảm rất nhiều công sức đãnh nhãn. ở DORI, mình thiết kế công cụ đánh nhãn để tối ưu cho bài toán văn bản đồng thời học hỏi và khắc phục những hạn chế của những công cụ có sẵn khác. 

Bước tốn thời gian nhất cho quá trình ocr là xác định và đánh nhãn cho từng từ, với những văn bản có vài trăm từ thì việc này cực kì tốn thời gian, ở DORI, sau khi các bạn upload ảnh lên mình sẽ tự chạy ocr để nhận dạng các từ/dòng/đoạn văn bản, đồng thời cũng nhận dạng nội dung của từng đó đó, giúp các bạn giảm thiếu khá nhiều thời gian huấn luyện. đồng thời, những mô hình nhận dạng và phát hiện văn bản cũng được huấn luyện trước trên vài trăm nghìn mẫu dữ liệu giúp cho các bạn chỉ cần đánh nhãn với số lượng rất ít, có thể là 10 ảnh, cũng được huấn luyện mô hình ra cho kết quả tốt. Ngoài ra, để rút ngắn thời gian đánh nhãn hơn nữa, DORI cũng cho phép sử dụng mô hình vừa huấn luyện của bạn để áp dụng vào tập dữ liệu chưa được đánh nhãn, vì mô hình mới này là do bạn tự huấn luyện trên tập các bạn đã đánh nhãn, nên kết quả của mô hình sẽ tốt hơn mô hình mặc định. 

Quay trở lại với các bước cụ thể trong quá trình đánh nhãn để huấn luyện mô hình phát hiện, nhận dạng và xác định thứ tự đọc của văn bản. 
### 1. Phát hiện, Nhận dạng và xác định thứ tự đọc của văn bản (Text Detection, Recognition & Reading Order)  

**Phát hiện văn bản là gì? (Text Detection)** : Đây là bước xác định các vùng có chứa văn bản trong ảnh hoặc tài liệu bằng cách tạo các hộp giới hạn (bounding boxes, box) xung quanh các từ

**Nhận dạng văn bản là gì? (Text Recognition)**: Sau khi xác định được vùng chứa văn bản, bước tiếp theo là nhận dạng nội dung trong các vùng đó, chuyển đổi hình ảnh chữ thành văn bản số mà máy tính có thể xử lý, thường sử dụng các mô hình OCR (Optical Character Recognition).

Có lẽ các bạn đã quen với việc cần phải xác định box và nội dung trong đó, 
Ở DORI, mình thiết kế công cụ đánh nhãn không chỉ giúp các bạn đánh từng từ dễ dàng mà còn cho phép xác định dòng/đoạn/page một cách dễ dàng thành thao tác **group**. Ở DORI, mình định nghĩa:
- **Dòng văn bản (Line)** là các từ có liên quan với nhau được sắp xếp theo tạo độ x ( ví dụ như tên sản phảm, địa chỉ, v.v.v tóm lại là những từ nên được gộp chung trên 1 dòng để tạo dành một cụm từ có nghĩa)
- **Đoạn văn bản(Paragraph)** là các dòng liên quan với nhau nên được sắp xếp theo tạo độ y để tạo ra một đoạn có nghĩa.
- **Trang văn bản(Page)*** là các tập hợp các từ/dòng/đoạn để tạo thành một trang, thông thường, các bạn chỉ cần nhóm tới level Paragraph là đủ, chỉ những file pdf/ảnh chứa nhiều trang mới cần tới level Page

Để hiểu rõ hơn các bạn xem những ví dụ ở dưới đây về việc nên nhóm dòng/đoạn/trang văn bản như nào cho tốt.

![image](https://github.com/user-attachments/assets/0ad88a4c-74ba-47df-91cc-5478b9fc2b1e)

Trong minh hoạ trên, các bạn thấy mình nên group các từ cộng -> hoà, độc lâp -> hạnh phúc, căn cưới công dân, v.v.v thành một dòng vì chúng là các cụm có nghĩa và để để đảm bảo thứ tự đọc chính xác 

![image](https://github.com/user-attachments/assets/9ccad852-b0c7-4d77-b41e-3e907dc97f7d)

Ở mình hoạ này, các bạn thấy file document có 2 trang rõ ràng, nên lúc này cần group các từ/dòng/đoạn tương ứng lại thành một trang. 
![image](https://github.com/user-attachments/assets/21b73210-6f4f-402d-82ed-4b591a8836c6)
Xem minh hoạ này, các bạn thấy nếu không group thành page( tương tự với dòng/đoạn) thì 2 từ nằm ở 2 trang sẽ nằm khác nhau sẽ nối với nhau vì thứ tự đọc mặc định sẽ là từ trái sang phải, trên xuống. Hãy tưởng tượng thông tin như *Tên Họ* nằm không liên tục thì chắn chắn sẽ ảnh hưởng đến độ chính xác nhận dạng key information. Do đó bước group này là quan trọng.

Các bước group ở trên giúp tạo thành hier text, từ đó giúp xác định thứ tự đọc đúng cho văn bản. Thứ tự đúng này ảnh hưởng rất lớn đến khả năng nhận dạng key information ở bước sau. ở DORI, mình đã phát triển mô hình giúp xác định đến 4 level (từ, dòng,đoạn, page) bằng một mô hình duy nhất. Hầu hết các mô hình có sẵn như google/aws chỉ có phép nhận dạng tới dòng hoặc page những kết quả không tốt tối với tiếng việt. Ngoài ra, theo mình quan sát, phần lớn các loại giấy tờ chỉ cần group tới paragraph là đủ để xác định thứ tự đọc. chỉ những văn bản có nhiều page như trên, thì các bạn mới xem xét group thêm page. 

Mặc dù mình chúng đã xác định dòng/đoạn/trang văn bản, tuy nhiên đôi khi thứ tự đọc vẫn không chính xác nếu chỉ sắp xếp các từ dựa vào những thông tin trên. Ví dụ như hoá đơn bị nghiên, hoặc văn bản có nhiều cột. thì lúc này chúng ta cần sử dụng mô hình reading order để xác định đúng thứ tự đọc của văn bản. ở DORI, mình cũng cho phép các bạn sắp xếp từng từ để xác định đúng thứ tự đọc văn bản, và dựa vào dữ liệu đó để huấn luyện reading order model. 

Phần tiếp theo mình sẽ làm rõ reading order model là gì?

**Xác định thứ tự đọc là gì? (Reading Order Detection)**  
Trong các tài liệu phức tạp, nếu chỉ dựa vào vị trí tạo độ và các nhóm như dòng/đoạn/trang thì thứ tự đọc vẫn không xác định được chính xác. Do đó các bạn cần phải xây dựng mô hình xác định thứ tự đọc, từ đó hỗ trợ việc trích xuất dữ liệu chính xác.

![image](https://github.com/user-attachments/assets/eee8f09a-bba2-481a-9642-961f798af1ed)


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

phải đợi text detection xong đã rồi mới tới text recognition, vì thứ tự huấn luyện là quan trọng
### Huấn luyện text detection như nào?
## Huấn luyện text recognition như nào?
## Huấn luyện reading order detection như nào?
## Huấn luyện key information extraction như nào?

### Huấn luyện Mô Hình để rút trích thông tin chính. 
Để huấn luyện mô hình rút trích thông tin chính, các bạn cần phải huấn luyện trước các mô hình sau **text detection**, **text recognition**, **reading order detection** ( nếu cần), và sau đó mới huấn luyện mô hình cuối cùng là **key information extraction**. Điều quan trọng là 4 mô hình này cần phải huấn luyện tuần tự, tức là mô hình **text detection** cần huấn luyện xong trước rồi mới tới mô hình **text recognition**, sau khi mô hình này xong thì mới huấn luyện tuần tự 2 mô hình còn lại. Thứ tự huấn luyện là qua trọng và chỉ bắt đầu huấn luyện mô hình tiếp theo nếu mô hình trước đó đã chạy xong. 

Để hiểu rõ hơn, vui lòng tham khảo video dưới đây.


## Kiểm Tra Mô Hình  
Sau khi mô hình được huấn luyện xong, hệ thống sẽ tự động triển khai (deploy) mô hình để cho phép bạn kiểm tra và sử dụng. Hãy chuyển sang tab **API**, chọn mô hình tương ứng, tải lên hình ảnh và nhấn **Done**. Kết quả sẽ hiển thị ở bên phải để bạn tham khảo. Đồng thời, bạn cũng có thể sử dụng lệnh **curl** để kiểm tra mô hình. Để rõ hơn, vui lòng tham khảo video dưới đây.

### Quy trình kiểm tra mô hình 
Thông thường sau khi train xong mô hình text detection và text recognition bạn đã có thể kiểm tra kết quả nhận dạng của mô hình bằng cách chon tab **API** sau đó chọn loại mô hình là **text recognition** sau đó upload một ảnh bạn muốn kiểm tra, chọn mô hình đã được huấn luyện, nhấn nút **Done** và đợi kết quả hiển thị bên phải. 
