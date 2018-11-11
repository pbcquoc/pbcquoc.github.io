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

{% highlight python linenos %}
base_model = applications.VGG16(weights='imagenet', include_top=False)
{% endhighlight %}

Với ảnh đầu vào có kích thước 1280x60 thì output của vgg16 là (nmaps, w, h) = ..., mỗi dòng tương ứng với chiều w thì tương ứng với một timstep cho tầng LSTM.

### Visual attention
Với mô hình CRNN, kết quả của vgg được truyền trực tiếp vào mô hình LSTM, tuy nhiên, với thực nghiệp của mình khi stack thêm một lớp attention ở giữa tầng vgg và LSTM sẽ cho kết quả nhận dạng tốt hơn. Attention cho phép model của chúng ta được thoải mái lựa chọn kết hợp thông tin giữa các timestep khác nhau để tổng hợp lại và sử dụng đặc trưng tổng hợp này làm đầu vào để nhận dạng chữ cái. Cụ thể vector context được tổng hợp tại mỗi timestep như sau:

<span class= "img-div" id="aligment_model" style="font-size:150%"></span>

Đầu tiên ta cần tính e với e chính là output của Aligment Model, một dạng feedforward nets với input là trạng thái của networks hiện tại 

<span class= "img-div" id="aligment_score" style="font-size:150%"></span>

Sau đó ta tính attention score tại mỗi timestep bằng hàm softmax vì chúng ta mong muốn tổng attention score bằng 1

<span class= "img-div" id="context_vector" style="font-size:150%"></span>

Cuối cùng, context vector là weighted average của trạng thái ẩn với attention score.

<script>
var alignment_model = $("#aligment_model");
katex.render("e_{ij}=a(s_{i-1}, h_{j})", alignment_model[0]);

var alignment_score = $("#aligment_score");
katex.render("\alpha_{ij} = softmax(e_{ij})", alignment_score[0]);

var context_vector = $("#context_vector");
katex.render("c_{i} = \sum_{j=1}^{T_{x}}\alpha_{ij}*h_{j}", context_vector[0]);
</script>

{% highlight python linenos %}
def attention_rnn(inputs):
    # inputs.shape = (batch_size, time_steps, input_dim)
    input_dim = int(inputs.shape[2])
    timestep = int(inputs.shape[1])
    a = Permute((2, 1))(inputs)
    a = Dense(timestep, activation='softmax')(a) // Alignment Model + Softmax
    a = Lambda(lambda x: K.mean(x, axis=1), name='dim_reduction')(a)
    a = RepeatVector(input_dim)(a)
    a_probs = Permute((2, 1), name='attention_vec')(a)
    output_attention_mul = multiply([inputs, a_probs], name='attention_mul') // Weighted Average 
    return output_attention_mul
{% endhighlight %}

### LSTM 
Với các vector context được tính ở tầng Attention được sử dụng là đầu vào cho mô hình LSTM. Tại mỗi timestep, chúng ta dự đoán từ tại thời điểm đó. Các timestep liên tục có thể dữ đoán cùng một từ.
<div class="img-div" markdown="0">
    <img src="/images/ocr_lstm.png" />
</div>
### Connectionist Temporal Classification loss
Với dữ liệu huấn luyện, chúng ta có nhãn là một đoạn text tương ứng với chữ trong bức ảnh đó. Chúng ta không có nhãn cụ thể tại mỗi thời điểm từ xuất hiện là gì tương ứng với timestep trong mô hình LSTM, do đó chúng ta không thể dùng cross entropy loss để tính độ lỗi mà phải dùng CTC loss trong bài toán bài. 

#### Encoding ground truth
CTC loss giải quyết vấn đề này theo cách rất là thông minh, cụ thể chúng ta sẽ thử tất cả các alignment của ground truth và tính score của tổng tất cả alignment. Alignment của ground truth được phát sinh bằng cách thêm blank token (-) và lặp lại bất kì kí tự nào trong ground truth.

Ví dụ ta có ground truth là: sun và có mô hình LSTM của chúng ta dự toán 4 timesteps. Thì những alignment đúng của ground truth là:
* sun -> -sun, s-un, su-n, sun-
* sun -> suun, ssun, sunn

Đới với những từ có 2 kí tự liên tục giống nhau, chúng ta sẽ thêm blank token để ở giữa để tạo ra một alignment đúng. Ví dụ với kí tự too. Các alignment đúng có thể là:
* too -> -to-o, tto-o

Nhưng ko thể là tooo.
#### Decoding text
Mô hình của chúng ta sẽ học để predict những alignment trên, sau đó chúng ta phải decode để đưa ra chuỗi dự đoán cuối cùng bằng cách gộp những kí tự lặp lại liên tiếp nhau thành một kí tự và sau đó xóa hết tất cả blank token.
Ví dụ với alignment tto-o thì sau khi decode chúng ta sẽ có too bằng cách gộp 2 kí tự 't' lại với nhau và xóa '-'.

#### Tính CTC loss
Với mỗi grouth truth chúng ta có nhiều alignment, bất kì alignment nào được dự đoán đều là một dự đoán đúng. Do đó, hàm loss ta cần tối ưu chính là tổng của tất cả các alignment.
