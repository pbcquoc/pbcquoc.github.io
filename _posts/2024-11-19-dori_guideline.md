---
layout: post
title: DRAFT_Hướng dẫn sử dụng DORI để nhận dạng các Key Information
---

Trong bài viết này, mình sẽ hướng dẫn bạn cách sử dụng DORI để nhận dạng và trích xuất các thông tin quan trọng từ tài liệu. Chẳng hạn, nếu bạn cần xác định Họ Tên và ID trên căn cước công dân, thì hai thông tin đó chính là mục tiêu mà bạn muốn mô hình nhận dạng. Về cơ bản, quy trình thực hiện gồm các bước chính sau:
1.	**Thu thập dữ liệu**: Giả sử bạn đã có sẵn tập dữ liệu đầy đủ. Nếu chưa, bạn cần tiến hành thu thập dữ liệu trước.
2.	**Đánh nhãn dữ liệu**: Đây là quá trình con người can thiệp để gắn nhãn các thông tin mà mô hình cần học. Để trích xuất thông tin chính, bạn cần đánh nhãn các thành phần:
 - Phát hiện văn bản
 - Nhận dạng văn bản
 - Xác định thứ tự đọc (không bắt buộc)
 - Trích xuất thông tin chính
3. **Huấn luyện mô hình**: Huấn luyện mô hình máy học trên tập dữ liệu đã được gán nhãn ở bước trên.
4. **Kiểm tra kết quả nhận dạng**: Đánh giá mô hình sau khi huấn luyện để xem kết quả đã đạt yêu cầu chưa. Nếu chưa, có thể thực hiện gán nhãn bổ sung và huấn luyện lại.

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

![image](/images/dori/create_project.jpg)

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

![image](/images/dori/hier_text_1.jpg)

Trong minh hoạ trên, các bạn thấy mình nên group các từ cộng -> hoà, độc lâp -> hạnh phúc, căn cưới công dân, v.v.v thành một dòng vì chúng là các cụm có nghĩa và để để đảm bảo thứ tự đọc chính xác 

![image](/images/dori/hier_text_2.jpg)

Ở mình hoạ này, các bạn thấy file document có 2 trang rõ ràng, nên lúc này cần group các từ/dòng/đoạn tương ứng lại thành một trang. 
![image](/images/dori/page.jpg)

Xem minh hoạ này, các bạn thấy nếu không group thành page( tương tự với dòng/đoạn) thì 2 từ nằm ở 2 trang sẽ nằm khác nhau sẽ nối với nhau vì thứ tự đọc mặc định sẽ là từ trái sang phải, trên xuống. Hãy tưởng tượng thông tin như *Tên Họ* nằm không liên tục thì chắn chắn sẽ ảnh hưởng đến độ chính xác nhận dạng key information. Do đó bước group này là quan trọng.

Các bước group ở trên giúp tạo thành hier text, từ đó giúp xác định thứ tự đọc đúng cho văn bản. Thứ tự đúng này ảnh hưởng rất lớn đến khả năng nhận dạng key information ở bước sau. ở DORI, mình đã phát triển mô hình giúp xác định đến 4 level (từ, dòng,đoạn, page) bằng một mô hình duy nhất. Hầu hết các mô hình có sẵn như google/aws chỉ có phép nhận dạng tới dòng hoặc page những kết quả không tốt tối với tiếng việt. Ngoài ra, theo mình quan sát, phần lớn các loại giấy tờ chỉ cần group tới paragraph là đủ để xác định thứ tự đọc. chỉ những văn bản có nhiều page như trên, thì các bạn mới xem xét group thêm page. 

Mặc dù mình chúng đã xác định dòng/đoạn/trang văn bản, tuy nhiên đôi khi thứ tự đọc vẫn không chính xác nếu chỉ sắp xếp các từ dựa vào những thông tin trên. Ví dụ như hoá đơn bị nghiên, hoặc văn bản có nhiều cột. thì lúc này chúng ta cần sử dụng mô hình reading order để xác định đúng thứ tự đọc của văn bản. ở DORI, mình cũng cho phép các bạn sắp xếp từng từ để xác định đúng thứ tự đọc văn bản, và dựa vào dữ liệu đó để huấn luyện reading order model. 

Phần tiếp theo mình sẽ làm rõ reading order model là gì?

**Xác định thứ tự đọc là gì? (Reading Order Detection)**  
Trong các tài liệu phức tạp, nếu chỉ dựa vào vị trí tạo độ và các nhóm như dòng/đoạn/trang thì thứ tự đọc vẫn không xác định được chính xác. Do đó các bạn cần phải xây dựng mô hình xác định thứ tự đọc, từ đó hỗ trợ việc trích xuất dữ liệu chính xác.

Ví dụ bên dưới minh hoạ cho bạn thấy thứ tự đúng của việc đọc là từ trên xuống trước, sau đó mới từ trái qua phải. Về cơ bản mô hình reading order xác định xem từ nào nên được đọc trước, từ nào nên được đọc sau. 
<div class="img-div" markdown="0">
    <img src="/images/dori/reading_order_example.jpg" width="500"/>
</div>
Để đánh nhãn cho mô hình reading order, DORI cho phép bạn di chuyển vị trí các từ để xác định lại thứ tự đọc, sau đó mô hình reading order dựa vào thông tin đó để học. 
![image](/images/dori/reading_order_tool.jpg)


Với DORI, mình tích hợp ba bước này trong một công cụ gọi là **Text Detection**. Công cụ này hiển thị các từ cùng với vị trí box của chúng. DORI cung cấp hai loại box cơ bản: **rectangle** và **polygon**. Với các văn bản thông thường, chỉ cần chọn **rectangle** là đủ, còn các chữ phức tạp mới cần dùng **polygon**.
![image](/images/dori/text_detection_tool.jpg)

Khi vẽ hộp xong, bạn sẽ được yêu cầu nhập văn bản, nội dung này sẽ hiển thị trong phần **document** bên trái và **reading order** bên phải để tiện theo dõi, group các từ thành dòng, dòng thành đoạn, và thành page tương ứng. thay đổi vị trí các từ để xác định đúng thứ tự đọc. 

DORI cho phép bạn select từng từ và right click để group lại thành dòng, các bạn có thể group từ và dòng thành đoạn, hay các dòng với nhau thành đoạn, tương tự như page. Đồng thời để dễ dàng hơn trong việc group mình hỗ trợ các bạn group các từ/dòng/đoạn trực tiếp tại phần hiển thị ảnh

Clip dưới là minh hoạ group các từ/dòng/đoạn bằng cách select từng từ/dòng
<div class="img-div" markdown="0">
    <img src="/images/dori/group.gif" width="500"/>
</div>
Clip dưới minh hoạ cách group từ/dòng/đoạn bằng cách select bên phần ảnh
<div class="img-div" markdown="0">
    <img src="/images/dori/group_2.gif" width="500"/>
</div>

DORI sẽ hiển thị màu khác nhau đối với dòng/đoạn/trang, đồng thời khi bạn select từng bên phần văn bản hay reading order thì các từ tương ứng bên phần ảnh sẽ được hiển thị giúp bạn kiểm tra nhanh chóng. Khi các bạn hover chuột qua các từ bên phần ảnh, nhãn của box đó sẽ được hiển thị giúp bạn kiểm tra, nếu bạn muốn thay đổi thì click vào box đó, thay đổi và nhấn enter. nhấn esc để ẩn box nhập liệu giúp bạn dễ dàng hơn khi điều chỉnh kích thước box. 

<div class="img-div" markdown="0">
    <img src="/images/dori/text_detection_label.gif" width="500"/>
</div>


### Làm như nào để đánh nhãn ở bước này chính xác?
Khi vẽ bounrady box cần phải vẽ cho từng từ, không phải vẽ cho từng kí tự, cũng ko phải vẽ cho từng câu hay đoạn. Boundary box cần bao phủ chính xác từng từ, ko đươc thiếu dấu câu hay nét, không được overlap với những boundary box khác. Các bạn xem thêm minh hoạ phía dưới, hình bên trái vẽ box chính xác bao phủ các dấu câu, các nét của từ. ngược lại bên trái các box vẽ thiếu chính xác, do đó sẽ ảnh hưởng kết quả nhận dạng 
![image](/images/dori/text_detection_example.jpg)

Sau khi đánh nhãn xong thì bước tiếp theo là xác định các key information mà bạn muốn nhận dạng. 
### 4. Trích xuất Thông tin Chính (Key Information Extraction)  
**Trích xuất thông tin chính là gì?**  
Sau khi văn bản được nhận dạng và sắp xếp, bạn cần xác định thông tin chính cần trích xuất. Ví dụ, nếu bạn cần rút trích **Họ Tên** và **ID**, bạn cần tạo nhãn cho hai loại thông tin này và chọn cụm từ tương ứng trong văn bản. Bạn tham khảo video dưới để biết chi tiết
<div class="img-div" markdown="0">
    <img src="/images/dori/kie_label.gif" width="500"/>
</div>


#### Làm như nào để đánh nhãn ở bước này chính xác?
Khi chọn các thông tin cần nhận dạng, các bạn phải select toàn bộ đối tượng đó trong văn bản. Ví dụ, bạn muốn đánh nhãn `Nguyễn Văn A` là tên thì phải bôi đen liên tục cụm từ này, không được select Nguyễn riêng, Văn riêng, A riêng. Nếu Nguyễn Văn A không nằm liên tục nhau trên văn bản, bạn cần quay lại bước text detection để group các từ thành dòng, dòng thành cụm. v.v hoặc thay đổi thứ tự đọc nếu việc group không hiệu quả.




## Huấn luyện Mô Hình  
Huấn luyện mô hình là quá trình sử dụng tập dữ liệu đã được đánh nhãn để xây dựng mô hình nhận dạng. Trên Dori, bạn có thể huấn luyện mô hình trên tập dữ liệu của mình bất cứ khi nào cần. Việc huấn luyện có thể được thực hiện sau mỗi 10, 50, 100, hoặc 1000 mẫu dữ liệu để theo dõi độ chính xác của mô hình, sau khi cập nhật hoặc bổ sung dữ liệu, hoặc khi muốn nhận dạng thêm trường thông tin mới.

Để bắt đầu huấn luyện trên Dori, bạn cần vào phần **Setting**, chọn **Train**, sau đó chọn mô hình phù hợp và cấu hình các tham số huấn luyện. Thông thường bạn sẽ không cần phải thay đổi tham số nào cả ngoại trừ `epoch_num` là số lần hoàn thành một lần huấn luyện trên toàn bộ dữ liệu. Điều chỉnh tham số này giúp giảm thời gian huấn luyện và giảm chi phí. Ngoài ra, tick chọn `Include AI-Augmented Labels` nếu bạn muốn thêm cả dữ liệu được phát sinh bởi model, tuy nhiên mình khuyến nghị không nên tick chọn vì dữ liệu này chưa được đánh nhãn lại cẩn thận bởi labeler.  Nhấn **Add** để khởi động quá trình huấn luyện mô hình. Hệ thống sẽ gửi email thông báo khi quá trình huấn luyện bắt đầu và kết thúc. Bạn có thể theo dõi lịch sử và độ chính xác của mô hình trong phần log.
![image](/images/dori/new_train.jpg)


Mỗi mô hình có một cấu hình riêng, bạn có thể click vào tab `Model` để xem cấu hình đang được sử dụng. Các thay đổi trong phần này sẽ được lưu lại, ngoài ra bạn cũng có thể reload lại default config của mô hình bằng nút reload bên phải. 
![image](/images/dori/model_screen.jpg)


Clip dưới minh hoạ quá trình tạo job train mô hình mới, các bạn có thể dễ dàng xem log và theo dõi độ chính xác của mô hình bằng cách click vào log. 
Phần log thể hiện các metrics quan trọng được highlight bằng màu đỏ, các metrics này càng lớn càng tốt. ngoài ra phần log cũng thể hiện các lỗi nếu có, do vậy bạn có thể nhìn vào log vào thực hiện các điều chỉ hoặc yêu cầu hỗ trợ. 

<div class="img-div" markdown="0">
  <img width="1396" alt="Screenshot 2024-12-10 at 21 41 44" src="/images/dori/ScreenRecording2024-12-10at21.46.58-ezgif.com-video-to-gif-converter.gif">
</div>

Sau khi huấn luyện xong, mô hình của bạn sẽ được tự động deploy ( xem thêm giải thích bên dưới). 
![image](/images/dori/train_screen.jpg)

Sau đó bạn có thể dễ dàng kiểm tra mô hình vừa huấn luyện bên tab `API`
![image](/images/dori/api_screen.jpg)


Lặp lại quy trình trên từ đánh nhãn, huấn luyện, kiểm tra lần lượt đối với text detection, rồi mới tới text recognition, reading order và cuối cùng là key information extraction vì kết quả huấn luyện của các mô hình trước đó được sử dụng cho các mô hình phía sau. 

Ở DORI, các bạn có thể dễ dàng huấn luyện model trên tập 10-20 ảnh, sau đó tăng dần lên, hãy tận dụng điều này để sớm có kết quả từ đó thực hiện các điều chỉnh hay tìm kiếm hỗ trợ từ mình hoặc liên hệ qua website của dori tại [dori.vn](https://dori.vn)

### Tip để tăng tốc quá trình đánh nhãn
Quá trình đánh nhãn thường mất nhiều thời gian và công sức, nhưng Dori cung cấp tính năng **AI-Augmented Labeling** để giúp bạn tận dụng mô hình được huấn luyện trên dữ liệu đã đánh nhãn để tự động áp dụng vào dữ liệu chưa đánh nhãn. Thông thường, bạn chỉ cần đánh nhãn 10-20 mẫu, sau đó huấn luyện mô hình trên dữ liệu này. Khi huấn luyện xong, bạn chỉ cần nhấn **AI-Augmented Labeling** để áp dụng mô hình lên phần dữ liệu còn lại chưa đánh nhãn. Cá nhân mình thường xuyên sử dụng tính năng này vì nó cực kì hữu dụng giúp tiết kiệm rất nhiều thời gian và tăng năng suất của cá nhóm.
![image](/images/dori/ai_augmented_labelling.jpg)


### Huấn luyện text detection, text recognition, reading order detection, key information extraction như nào?
Huấn luyện mô hình text detection, text recognition, v.v trên DORI về cơ bản là hoàn thành giống như. Tuy nhiên mình muốn nhấn mạnh rằng thứ tự huấn luyện là quan trọng vì mô trước phía sau như text recognition, key information extract sử dụng mô hình huấn luyện ở bước trước. 

## Chạy Offline mô hình của bạn. 

Khi huấn luyện xong, DORI cho phép bạn tải về mô hình của mình và chạy offline trên máy tính, những mô hình được mình thiết kế để chạy on-device, lightweight nên đều chạy được trên CPU và có tốt độ xử lý thuộc top đầu trên thị thường. Bạn chỉ đơn vào mô hình đã huấn luyện để tải về và chạy theo hướng dẫn trong README.
![image](/images/dori/download_model.jpg)



Ngoài các chức năng ở trên DORI cho phép các bạn thêm các thành viên để đánh nhãn, và cũng hỗ trợ các bài toán quan trọng khác như Phân loại văn bản, xác định layout, chỉnh phẳng ảnh, nhận dạng và xác định cấu trúc bảng, rút trích mối quan hệ. 
