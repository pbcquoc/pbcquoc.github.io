---
layout: post
title: VietOCR - Nhận Dạng Tiếng Việt Sử Dụng Mô Hình Transformer
---

# Giới Thiệu

<div class="img-div-any-width" markdown="0">
    <img src="/images/vietocr/sample.png" />
</div>
Trong blog này, mình giới thiệu 2 mô hình cho bài toán nhận dạng chữ tiếng việt: AttentionOCR và TransformerOCR. AttentionOCR sử dụng kiến trúc attention seq2seq đã được sử dụng khá nhiều trong các bài toán NLP và cả OCR, còn TransformerOCR sử dụng kiến trúc của Transformer đã đạt được nhiều tiến bộ vượt bậc cho cộng đồng NLP. 
Một câu hỏi mà mình cũng khá quan tâm là *Liệu TransformerOCR có mang lại kết quả vượt bậc như những gì các bạn đã nhìn thấy trong các bài toán NLP hay không ?*

Đồng thời, mình cũng cung cấp một [thư viện](https://github.com/pbcquoc/vietocr) mới cho bài toán OCR, thư viện hướng tới kết quả chính xác, nhanh chóng, dễ huấn luyện, dễ dư đoán cho cả các bạn có nhiều kinh nghiệm cũng có thể sử dụng được trong các bài toán liên quan đến số hóa.

# Mô Hình 
Trong phần này mình sẽ trình bày chi tiết cách kết hợp mô hình CNN và mô hình Language Model (Seq2Seq và Transformer) để tạo thành một mô hình giúp các bạn giải quyết bài toán OCR. Ngoài ra, mình cũng so sánh hạn chế của mô hình OCR cổ điển sử dụng CTCLoss với 2 mô hình kể trên từ đó giúp các bạn lựa chọn mô hình phù hợp trong các vấn đề thực tế. 

## AttentionOCR

<div class="img-div-any-width" markdown="0">
    <img src="/images/vietocr/cnn_seq2seq.jpg" />
</div>

## TransformerOCR

<div class="img-div-any-width" markdown="0">
    <img src="/images/vietocr/transformerocr.jpg" />
</div>

# Huấn Luyện

# Inference 

# Kết Quả

# Hướng dẫn sử dụng thư viện
## Huấn luyện mô hình trên tập dữ liệu của các bạn
## Dự đoán 
## Model Zoo


(to be continued)
