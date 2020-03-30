---
layout: post
title: Transformer - Ngươi Không Phải Là Anh Hùng, Ngươi Là Quái Vật Nhiều Đầu.
---


<div class="img-div-any-width" markdown="0">
    <img src="/images/transformer/transformer.jpg" />
</div>
Trong blog này, mình sẽ trình bày chi tiết cách mô hình Transformer hoạt động, cũng như là cách cài đặt mô hình chi tiết cho những bạn mới có kiến thức cơ bản về deep learning như CNN hoặc LSTM cũng có thể hiểu được.<br/><br/>Sự nổi tiếng của mô hình Transformer thì không cần phải bàn cãi, vì nó chính là nền tảng của rất nhiều mô hình khác mà nổi tiếng nhất là BERT (Bidirectional Encoder Representations from Transformers) một mô hình dùng để học biểu diễn của các từ tốt nhất hiện tại và đã tạo ra một bước ngoặc lớn cho động đồng NLP trong năm 2019. Và chính Google cũng đã áp dụng BERT trong cỗ máy tìm kiếm của họ.<br/><br/>Ý tưởng chủ đạo của Transformer vẫn là áp dụng cơ thể Attention, những ở mức phức tạp hơn và thật sự là thú vị hơn so với cách được đề xuất trước đó trong một [bài báo](https://arxiv.org/abs/1508.04025) của tác giả Lương Minh Thắng, một người Việt rất nổi tiếng trong cộng đồng deep learning. 

# Tổng Quan Mô Hình
Để cho dễ cảm nhận được cách mà mô hình hoạt động, mình sẽ trình bày trước toàn bộ kiến trúc mô hình ở mức high-level và sau đó sẽ đi chi tiết từng phân nhỏ cũng như công thức toán của nó. 

Giống như những mô hình dịch máy khác, kiến trúc tổng quan của mô hình transformer bao gồm 2 phần lớn là encoder và decoder. Encoder dùng để học vector biểu của câu với mong muốn rằng vector này mang thông tin hoàn hảo của câu đó. Decoder thực hiện chức năng chuyển vector biểu diễn kia thành ngôn ngứ đích.

Trong ví dụ ở dưới, encoder của mô hình transformer nhận một câu tiếng việt, và encode thành một vector biểu diễn ngữ nghĩa của câu <i>little sun</i>, sau đó mô hình decoder nhận vector biểu diễn này, và dịch nó thành câu tiếng việt <i>mặt trời bé nhỏ</i>

<div class="img-div" markdown="0">
    <img src="/images/transformer/overview.jpg" />
</div>

Một trong nhưng ưu điểm của transformer là mô hình này có khả năng xử lý song song cho các từ. Như các bạn thấy, Encoders của mô hình transfomer là một dạng feedforward neural nets, bao gồm nhiều encoder layer khác, mỗi encoder layer này xử lý đồng thời các từ. Trong khi đó, với mô hình LSTM, thì các từ phải được xử lý tuần được. 

<div class="img-div-any-width" markdown="0">
    <img src="/images/transformer/overview2.jpg" />
</div>

# Encoder
## Embedding Layer
## Positional Encoding
## Self Attention Layer
## Multi Head Attention
# Decoder
## Masked Multi Head Attention
# Loss function
# Implemetation
(to be continuted)
