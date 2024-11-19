---
layout: post
title: Hướng dẫn sử dụng Dori để nhận dạng các Key Information
---
<div class="img-div" markdown="0" >
    <img src="https://github.com/user-attachments/assets/09195ae2-80ae-4e3b-8f40-9237f812ff1c" width="800px" height="500px"/>
</div>
Trong bài viết này, chúng tôi sẽ hướng dẫn bạn cách sử dụng Dori để nhận dạng và rút trích các thông tin chính (Key Information). Ví dụ, nếu bạn cần nhận dạng thông tin như `họ tên` và `ID` trên căn cước công dân, thì `họ tên` và `ID` chính là những thông tin cần được rút trích. Để thực hiện điều này, bạn sẽ cần trải qua các bước sau:-
- Phát hiện văn bản
- nhận dạng văn bản
- xác định thứ tự đọc
- rút trích thông tin chính
  
Các bước trên các bạn có thể làm dễ dàng bằng cách thao tác trên dori, dori cung cấp sẵn các chức năng đánh nhãn, huấn luyện, deploy, test model, tăng tốc quá trình đánh nhãn, huấn luyện, bằng những mô hình có sẵn cho tiếng việt. Mình sẽ mô tả các bước thực hiện cụ thể ở phần sau, trước mắt các bạn cần tạo một project mới trên dori bằng cách click vào `Create Project`, sau đó điền thông tin tên project, mô tả, phân loại, các chọn 2 tool cơ bản là `text detection` và `key information extraction`, sau đó click vào upload ảnh và chọn các ảnh cần đánh nhãn lên. Mình gợi ý là nên upload tầm 20 ảnh cho toy project, 100-300 ảnh để có bắt đầu huấn luyện mô hình tốt cho chạy thực tế. Các bạn không cần phải upload hết các ảnh từ đầu mà có thể bổ sung sau này trong phần `setting` của project. Sau khi upload ảnh xong, hệ thống sẽ tự chạy mô hình text detection mặt định để hỗ trợ các bạn giảm thời gian đánh nhãn. 

<img width="1424" alt="Screenshot 2024-11-19 at 15 20 06" src="https://github.com/user-attachments/assets/f3e2d424-9cf0-46e1-9580-6d4a2010c85d">




Sau khi tạo xong project, các bạn cần click vào nút `label` để tạo phần đánh nhãn cho các ảnh vừa upload. 

### 1. Phát hiện và nhận dạng Văn Bản (Text Detection & Recognition)

Mình mô tả quy trình 3 bước trong phát hiện văn bản cho các bạn hiểu được mục đích của những bước này dùng để làm gì trước khi đi vào hướng dẫn cách sử dụng dori để giải quyết bài toán này. 

**Phát hiện văn bản là gì? (Text detection)**  
Phát hiện văn bản là bước đầu tiên trong việc xử lý tài liệu. Nó xác định các vùng có chứa văn bản trong ảnh hoặc tài liệu. Quá trình này tạo ra các hộp giới hạn (bounding boxes) xung quanh từng đoạn, dòng, hoặc cụm từ cần xử lý thêm. 

**Nhận dạng văn bản là gì? (Text Recognition)**  
Sau khi xác định vùng chứa văn bản, bước tiếp theo là nhận dạng nội dung của văn bản trong các vùng đó. Quá trình này chuyển đổi hình ảnh của chữ thành dạng văn bản kỹ thuật số mà máy tính có thể đọc được, thường sử dụng các mô hình OCR (Optical Character Recognition).


**Xác định thứ tự đọc là gì? (Reading Order Detection)**  
Trong nhiều tài liệu, thứ tự đọc của văn bản không phải lúc nào cũng rõ ràng, đặc biệt khi có nhiều cột, bảng, hoặc bố cục phức tạp. Việc xác định thứ tự đọc đảm bảo rằng thông tin sẽ được xử lý theo đúng thứ tự mong muốn, giúp việc rút trích dữ liệu chính xác hơn.


Trong dori 3 bước text detection, text recognition, reading order detection được gọp chung trong một tool gọi là text detection như hình dưới. Ở tool này, sẽ hiển thị các từ và vị trí box của từ đó. Dori mặc định sẽ xứ lý văn bản của bạn theo đơn vị là từng từ. Do đó các bạn cần đánh nhãn theo từng từ, các bạn cần sửa lại, hay xoá hay vẽ thêm các box và text đã có sẵn cho chính xác và xác định lại thứ tự đọc cho chính xác (nếu cần thiết). Dori cung cấp 2 loại box chính là rectangle và polygon để vẽ box, thông thường chỉ cần chọn rectangle box là đủ, các chữ phức tạp mới cần pology, sau khi vẽ box xong, dori sẽ yêu cầu nhập text, các bạn cần nhập chính xác text hiển thị, phần text này sẽ hiển thị lên phần document bên trái và reading order bên phải để các bạn theo dõi. 

https://github.com/user-attachments/assets/57cc6990-0865-40dd-96db-64015fa07f3d

Các từ sau khi nhận dạng có thể cần được sắp xếp lại theo đúng thứ tự đọc để giúp rút trích key information chính xác hơn, để sắp xếp lại các từ các bạn cần select từ cần đổi vị trí trên pannel bên phải drag & drop đến vị trí chính xác.

https://github.com/user-attachments/assets/833b00f1-a228-4474-b614-aeaaa747fae8

### 4. Rút Trích Thông Tin Chính (Key Information Extraction)
**Rút trích thông tin chính là gì?**  
Sau khi văn bản được nhận diện và sắp xếp, bạn cần xác định các thông tin chính mà bạn cần. Ví dụ, bạn cần rút trích `họ tên` và `ID` thì cần 2 loại nhãn này như bên dưới. sau đó select cụm từ là `họ tên` và `ID` trong văn bản trên trái. 


<iframe width="1000" height="600" src="https://www.youtube.com/embed/7hs8inkl2CE?si=XpVReG2fPNvpTtwx" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## Sử dụng tính năng self train để tăng tốc quá trình đánh nhãn

## Huấn luyện mô hình 

## Kiểm tra mô hình 
