---
layout: post
title: Huấn luyện mô hình CRNN cho nhận dạng chữ viết tay Tiếng Việt - How to train your dragon. 
---
# Giới Thiệu
Nhận dạng chữ tiếng việt là một trong những vấn đề rất quan trọng và có nhiều ứng dụng trong lĩnh vực số hóa văn bản để lưu trữ, tìm kiếm trên các văn bản scan, ảnh. Mô hình Convolutional Recurrent Neural Network là mô hình phổ biến cho kết quả rất khả quan trọng việc nhận dạng chữ in cũng như chữ viết tay. <br/> Trong blog này mình hướng dẫn các bạn huấn luyện một mô hình CRNN cho các bài toán nhận dạng chữ Tiếng Việt bằng mã nguồn mình cung cấp sẵn tại [đây](https://github.com/pbcquoc/crnn). Đồng thời, mình cũng cung cấp bộ dataset tự phát sinh gồm 2m ảnh và mô hình đã được huấn luyện sẵn trên tập dataset này. Mô hình này đạt kết quả khá tốt, và được mình sử dụng trong một số dự án thực tế. 

# Mô hình
Ở phần này mình chỉ trình bày tóm tắt kiến trúc mô hình. 
<div class="img-div" markdown="0">
    <img src="https://github.com/pbcquoc/crnn/raw/master/img/crnn.png" />
</div>

Mô hình CRNN cho bài toán nhân dạng chữ viết tay trong cài đặt này là mô hình đơn giản bao gồm 2 phần: CNN và LSTM. Cụ thể CNN sẽ rút trích feature từ ảnh,kết quả sẽ cho ra tensor 3 chiều có kích thước batch_size x 1 x f. Do đó, các bạn cần lưu ý kiến trúc của CNN phải phù hợp để có thể nhận đầu vào có kích thước wxh và chiều w của tensor output có kích thước là 1. Để đưa tensor output vào LSTM, các bạn đơn giản chỉ cần remove w có kích thước là 1. Đến đây, các bạn áp dụng mô hình LSTM và dùng CTC loss để cập nhật trọng số. 

# Chuẩn bị dữ liệu
# Huấn luyện
# Kết quả
# Pretrained model và Dataset

(to be continued)
