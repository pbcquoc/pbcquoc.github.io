---
layout: post
title: Nhận Dạng Chữ Tiếng Việt - Vietnamese OCR
---
### Giới thiệu
Nhận dạng chữ viết tay là một trong những bài toán rất thú vị, với đầu vào là một ảnh chứa chữ và đầu ra là chữ chứa trong ảnh đó. Bài toán này tương đối khó đối với chữ viết tay, cộng với viết bộ dữ liệu việt nam tương đối hiếm có. Lần này, mình sẽ chia sẽ hướng tiếp cận mà team mình đã sử dụng trong cuộc thi do Cinnamon tổ chức, nhờ những cách sử lý hợp lý mà team mình đã đạt đượt top 1 cuộc thi này. Nhưng mà cuộc thì này tổ chức cũng dài hơi, làm những thí sinh hơi nản. (tầm 2 tháng với phần thi + bootcamp). Sơ qua về một chút ứng dụng của OCR. Mình thấy có 2 ứng dụng khá nổi tiếng là Google Translate, với GotIt của Hùng Trần.

### Dữ liệu
Dữ liệu của bài toán này là chỉ có text-line không có scene text nên vấn đề đơn giản hơn nhiều so với việc phải detect được chữ trên ảnh có background như ngoài thực tế. 
![dữ liệu]({{site.baseurl}}/images/ocr_dataset.png)


## Chuẩn bị dữ liệu
Đối với văn bản scan thì việc tiền xử lý như remove noise,background là cực kì quan trọng và ảnh hưởng khá nhiểu đến độ chính xác của bài toán. Để remove noise và background thì các bạn có thể sử dụng kmean để cluster bức ảnh ra 2 màu chủ đạo trắng và đen rồi sau đó binary ảnh dựa vào kết quả cluster. Trong quá trình xử lý các bạn có thể sử dụng [tool](https://github.com/mauvilsa/imgtxtenh) này nhé

<div class="img-div" markdown="0">
    <img src="/images/ocr_process_step.png" />
</div>

## Mô hình
Dữ liệu đã chuẩn bị xong thì đến phần model. Môt trong những mô hình được hay sử dụng là CRNN, tuy nhiên mô hình mình sử dụng thêm vào cơ chế attention cho phép model lựa chọn vùng ảnh mong muốn để phát sinh ra text. Cơ chế attention được sử dụng rất nhiều trong machine translation. Mình sẽ có một bài về cơ chế này,tuy nhiên các bạn có thể tham khảo [tại đây](http://www.wildml.com/2016/01/attention-and-memory-in-deep-learning-and-nlp/). 

<div class="img-div" markdown="0">
    <img src="/images/ocr_crnn.png" />
</div>

Đối dữ liệu là ảnh thì chúng ta sẽ dùng mô hình CNN để extract feature.  Ở đây, mình dùng VGG16 nhé. Trong model hình này, chúng ta nên lưu ý số tầng pooling, mình chỉ sử dụng 4 tầng pooling của VGG16, mỗi tầng pooling sẽ có kích thước 2x2,đồng thời bỏ hết tất cả các tầng fully connected cuối cùng, do đó output của VGG16 là một tập các feature maps, mỗi pixel trên feature tương ứng vùng 16x16 trên bức ảnh đầu vào. 

<div class="img-div" markdown="0">
    <img src="/images/ocr_pooling_size.png" />
</div>

Việc chọn kích thước và số tầng pooling này cực kì quan trọng vì nó ảnh hưởng đến số pixel mà mỗi timestep nhìn thấy được.Nếu các bạn chọn kính thước tầng pooling size quá lớn sẽ dần đến việc một step sẽ bao gồm nhiểũ chữ trong ảnh do đó mô hình sẽ không nhận dạng được.

```python
def maxpooling(base_model):
    model = Sequential(name='vgg16')
    for layer in base_model.layers[:-1]:
        if 'pool' in layer.name:
            pooling_layer = MaxPooling2D(pool_size=(2, 2), name=layer.name)
            model.add(pooling_layer)
        else:
            model.add(layer)
    return model
```
