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

## CNN của mô hình OCR
Mô hình CNN dùng trong bài toán OCR nhận đầu vào là một ảnh, thông thường có kich thước với chiều dài lớn hơn nhiều so với chiều rộng, do đó việc điều chỉnh tham số stride size của tầng pooling là cực kì quan trọng, thông thường kích thước stride size của các lớp pooling cuối cùng là wxh=2x1 trong mô hình OCR. Không thay đổi stride size phù hợp với kích thước ảnh, kết quả của mô hình sẽ tệ. 

Đối với mô hình VGG, việc thay đổi pooling size khá dễ do kiến trúc đơn giản, tuy nhiên đối với mô hình phức tạp khác như resnet việc điều chỉnh tham số pooling size hơi phức tạp do một ảnh bị downsampling ko chỉ bởi tầng pooling mà còn tại các tầng convolution khác.  

Trong pytorch, đối với mô hình VGG, các bạn chỉ đơn giản là thay thế stride size của tầng pooling.
```
cnn.features[i] = torch.nn.AvgPool2d(kernel_size=ks[pool_idx], stride=ss[pool_idx], padding=0)
```

## AttentionOCR

<div class="img-div-any-width" markdown="0">
    <img src="/images/vietocr/cnn_seq2seq.jpg" />
</div>

AttentionOCR là sự kết hợp giữa mô hình CNN và mô hình Attention Seq2Seq. Một ảnh qua mô hình CNN, sẽ cho một feature map có kích thước channelxheightxwidth, feature map này sẽ trở thành đầu vào cho mô hình LSTM, tuy nhiên, mô hình LSTM chỉ nhận chỉ nhận đầu vào có kích thước là hiddenxtime_step. Một cách đơn giản và hợp lý là 2 chiều cuối cùng heightxwidth của feature map sẽ được đuổi thẳng. Feature map lúc này sẽ có kích thước phù hợp với yêu cầu của mô hình LSTM.

<div class="img-div-any-width" markdown="0">
    <img src="/images/vietocr/cnn_fts.jpg" />
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
