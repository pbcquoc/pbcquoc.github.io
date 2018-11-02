---
layout: post
title: Nhận Dạng Chữ Tiếng Việt - Vietnamese OCR
---
### Giới thiệu
Nhận dạng chữ viết tay là một trong những bài toán rất thú vị, với đầu vào là một ảnh chứa chữ và đầu ra là chữ chứa trong ảnh đó. Bài toán này tương đối khó đối với chữ viết tay, cộng với viết bộ dữ liệu việt nam tương đối hiếm có. Lần này, mình sẽ chia sẽ hướng tiếp cận mà team mình đã sử dụng trong cuộc thi do Cinnamon tổ chức, nhờ những cách sử lý hợp lý mà team mình đã đạt đượt top 1 cuộc thi này. Nhưng mà cuộc thì này tổ chức cũng dài hơi, làm những thí sinh hơi nản. (tầm 2 tháng với phần thi + bootcamp). Sơ qua về một chút ứng dụng của OCR. Mình thấy có 2 ứng dụng khá nổi tiếng là Google Translate, với GotIt của Hùng Trần.

### Dữ liệu
Dữ liệu của bài toán này là chỉ có text-line không có scene text nên vấn đề đơn giản hơn nhiều so với việc phải detect được chữ trên ảnh có background như ngoài thực tế. 
![dữ liệu]({{site.baseurl}}/images/ocr_dataset.png)

Môt trong những mô hình được hay sử dụng là CRNN, tuy nhiên mô hình mình sử dụng thêm vào cơ chế attention cho phép model lựa chọn vùng ảnh mong muốn để phát sinh ra text. Cơ chế attention được sử dụng rất nhiều trong machine translation. Mình sẽ có một bài về cơ chế này,tuy nhiên các bạn có thể tham khảo [tại đây](http://www.wildml.com/2016/01/attention-and-memory-in-deep-learning-and-nlp/). 

![Model mà team đã sử dụng]({{site.baseurl}}/images/ocr_crnn.png)
