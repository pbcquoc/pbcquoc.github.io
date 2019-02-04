---
layout: post
title: Tìm Hiểu và Áp Dụng Cơ Chế Attention
---

## Giới thiệu
Theo thông lệ mình sẽ giới thiệu sơ qua cơ chế attention là gì, lịch sử, những cột mốc từ khi attention được ứng dụng. Tuy nhiên, do mình thấy rằng một số bạn nghĩ rằng cơ chế attention khá phức tạp nên trước hết mình muốn nhấn mạnh rằng: Cơ chế attention chỉ đơn giản là trung bình có trọng số của những "thứ" mà chúng ta nghĩ nó cần thiết cho bài toán, điều đặc biệt là trọng số này do mô hình tự học được. Cụ thể, trong bài toán dịch máy ở ví dụ dưới, khi sử dụng cơ chế attention để phát sinh từ **little**, mình sẽ cần tính một vector context *C* là trung bình có trọng số của vector biểu diễn các từ **mặt, trời, bé, nhỏ** tương ứng với vector h1,h2,h3,h4 rồi sử dụng thêm vector context *C* này tại lúc dự đoán từ **little**, và nhớ rằng, trọng số này là các số scalar, được mô hình tự học. Các bạn đã thấy cơ chế attention thật đơn giản rồi nhỉ. Vậy chúng ta hãy bắt đầu đi vào chi tiết.
<div class="img-div" markdown="0">
    <img src="/images/attn_seq2seq.png" />
</div>

## Tổng quan
Theo thực tế, mình nghĩ bộ não của chúng ta cũng có cơ chế attention của riêng nó. Ví dụ như, mắt của chúng ta có tầm nhìn [120](https://en.wikipedia.org/wiki/Field_of_view) độ theo cả chiều thẳng đứng và ngang. Tuy nhiên, tại một thời điểm, hầu như chúng ta chỉ xử lý một phần nhỏ thông tin của bức ảnh. Bạn có để ý khi chúng ta lái xe, lúc rẽ trái, hay phải, chúng ta chỉ chú ý vào một phần không gian trên kính chiếu hậu mà thôi, rồi từ đó mới xử lý để đưa ra quyết định di chuyển tiếp theo. Cơ chế này của bộ não giúp chúng ta không cần nhiều năng lượng để đưa ra quyết định tuy nhiên vẫn cho kết quả tin cậy. 

Có khá nhiều loại cơ chế attention khác nhau, tuy nhiên tổng quan chúng ta có 2 loại:
* Hard Attention: sử dụng [reinforcement learning](http://proceedings.mlr.press/v37/xuc15.pdf) để học vị trí cần phải chú ý
* Soft Attention: học bộ trọng số bằng thuật toán backpropagation
Với hard attention, 

## Chi tiết cơ chế Attention

## Áp dụng

### Trong mô hình seq2seq

### Trong mô hình phân tích ngữ nghĩa - sentiment analysis

## Cài đặt thuật toán

