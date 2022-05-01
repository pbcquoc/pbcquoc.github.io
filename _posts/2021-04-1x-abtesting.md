---
layout: post
title: A/B Testing - Lý thuyết cơ bản và cách áp dụng
---

# Tại sao chúng ta cần A/B Testing
Dưới góc nhìn của chúng ta, hầu hết các sự kiện trong cuộc sống đều có tính ngẫu nhiên. Tính ngẫu nhiên này có thể là việc quan sát của chúng ta bị hạn chế, hoặc bản chất của việc đó là ngẫu nhiên. Để rõ hơn mình sẽ đưa ra 2 ví dụ sau:

Ví dụ đầu tiên để minh họa tính ngẫu nhiên là do việc quan sát của chúng ta bị hạn chế. 
- Giả sử mình muốn tính vị trí sau 3 giây của hòn đá lăn trên một con dốc. Vì hạn chế của việc đo đếm các yếu tố tác động đến hòn đá như độ dốc, lực cản, hình dáng con dốc mà mình chỉ có thể tính toán được vị trí tương đối chính xác của hòn đá mà thôi. Vị trí của hòn đá sau 3 giây có thể nằm ở vị trí xung quanh vị trí mà mình ước lượng. Nếu mình có khả năng đo đếm các yếu tố ảnh hưởng chính xác tuyệt đối thì mình có thể tính toán vị trí chính xác theo các quy tăc vật lý.

Ví dụ tiếp theo là minh họa bản chất của việc đó là ngẫu nhiên. 
- (Cần bổ sung)

Dưới góc độ của chúng ta, việc ngẫu nhiên cho dù đến từ bản chất của sự việc hay do hạn chế quan sát, thường không quan trọng, cái chúng ta cần là công cụ để xem xét, đánh giá, so sánh giữ các sự kiện này. 

Quay lại với các vấn đề thực tế của chúng ta, thông thường trong các công ty có phát triển sản phẩm các sản phẩm riêng như Zalo, Tiki, Momo, ..., những ứng dụng của công ty này thường được phát triển lên từ từ, được thêm nhiều tính năng mới, cải thiện tính năng cũ, cải thiện tốc độ phản hồi. Việc bổ sung, cải thiện những tính năng này là hành động chủ quan, có thể làm cho ứng dụng tệ đi hoặc tốt hơn theo nghĩa nào đó ví dụ như ít hoặc nhiều người sử dụng app hàng ngày, ít hoặc nhiều giao dịch. Do đó, chúng ta cần có cách nào đó để kiểm tra xem liệu tính năng mới đó có thực sự tốt hơn như chúng ta nghĩ hay không?

# Một phương pháp kiểm tra đơn giản và không chính xác.
Để giải quyết câu hỏi liệu tính năng mới có thực sự tốt hơn hay không? Chúng ta thường nghĩ đến phương pháp kiểm tra sai lầm như sau: Cứ mở tính năng mới cho tất cả khách hàng sử dụng, sau đó quan sát xem liệu các chỉ số như số khách sử dụng hằng ngày có tăng hay không, (và các chỉ số quan trọng khác tùy mô hình kinh doanh). Nếu thấy tăng thì chúng ta bảo là tốt hơn, và cứ thế tự tin rằng mình đang làm điều tốt đẹp. 

Phương pháp này tuy sai, nhưng lại đơn giản nên hầu như ai cũng nghĩ được và vì thế nên lại thường được sử dụng (vì người đó không biết sai ở chỗ nào). Nhưng mình nhấn mạnh lại một lần nữa là phương pháp này sai vì những vấn đề không được giải quyết sau:
- Trong thời gian bạn chạy tính năng mới, nhưng yếu tố khác tác động vào làm tăng chỉ số mà bạn đang đo. Ví dụ như bạn đang kiểm tra xem giao diện mới có làm tăng lượt click vào sản phẩm so với giao diện cũ hay không thì bộ phận khác của công ty tăng tiền quảng cáo, tạo ra các mã giảm giá, khuyến mãi cho khách hàng thì có thể chỉ số bạn đang theo dõi cũng tăng theo mặc dù cái giao diện mới chả tạo ra thêm lượt click so với cái giao diện cũ, hoặc các yếu tố ngoài xã hội như dịch bệnh, thời tiết, chính sách nhà nước tác động vào làm tăng hoặc giảm chỉ số đó. Tóm lại có quá nhiều yếu tố khác tác động vào đúng lúc mà bạn đang kiểm định tính năng mới làm cho việc tăng hoặc giảm chỉ số theo phương pháp này không đáng tin cậy. 

Hậu quả của việc các chỉ số thống kê không đúng là chúng ta không có có sở đánh giá mức độ cải tiến, chọn nhầm các tính năng tệ hại thay vì tốt thực sự, những ý tưởng rác lại được hoang nghênh.

# Chiến lượt ABtest ngây thơ - Cứ nghĩ là AB Test, những lại không phải !
Có nhiều bạn cứ nghĩ rằng AB Test là việc chia tập khách hàng thành 2 tập baseline và variant theo cách ngẫu nhiên có kích thước tương đương nhau. Rồi chạy thí nghiệm, tập baseline thì vẫn giữ model gốc, còn tập variant thì chạy model cải tiến. Rồi sau đó, chỉ cần đơn giản là so sánh các chỉ số business quan trọng trên 2 tập đó là xong, nếu thấy tập variant có giá trị tốt hơn thì chứng tỏ model cải tiến tốt hơn thật, ngược lại thì model cải tiến tệ hơn. 

**Điều này là một sự sai lầm tệ hại!**

Để hiểu tại sao lại sai lầm, mình sẽ chuyển sang phần tiếp theo để mục tiêu thật sự của ABTest là gì? và thấy được cách mình vừa trình bày ở trên là sai lầm tệ hại. 

# Làm rõ mục tiêu của việc ABTest
Bạn đã quên mất 2 điều mà khi bạn quên 2 điều này làm cho bạn cứ nghĩ rằng mình vẫn đang làm đúng.
- Mục tiêu của việc kiểm định là để chứng minh model cải tiến tốt hơn trên toàn bộ tập khách hàng. Tập khách hàng này là tập khách hàng tại mọi thời điểm, tại tương lai, hiện tại, và quá khứ. 
Câu phát biểu này phát sinh vấn đề là vậy thì tập khách hàng tương lai ở đâu chứ? Chúng ta chưa có. Vậy thì làm sao để có tập khách hàng tương lai này chứ? chúng ta sẽ chờ cho công này phá sản. Vậy thì toàn bộ tập khách hàng của công ty là từ lúc công ty khởi nghiệp đến lúc phá sản. Lúc này đây, bạn dùng **chiến lượt ABTest ngây thơ** ở trên để tính toán thì bạn làm đúng. 

Nhưng mà công ty này phá sản thì ai cần bạn làm gì nữa chứ? Vậy giải pháp là gì?

Chắc chắn rằng chúng ta vẫn phải đạt được mục tiêu là chứng minh rằng model cải tiển tốt hơn trên toàn bộ tập khách hàng, nhưng thay vì phải đợi công ty phá sản để có được toàn bộ tập khách hành, chúng ta sẽ **sampling** tập khách hàng để đem ra kiểm định, và đó chính là tập khách hàng hiện tại và cũng vì chúng ta **sampling** một tập nhỏ hơn nên các con số chúng ta tính toán là biến ngẫu nhiên. Vì là biến ngẫu nhiên nên chúng ta có thể thấy chỉ số của mô hình cải tiến tốt hơn, hoặc tệ hơn nhưng không có nghĩa là chúng tốt/tệ hơn thật. Đơn giản chỉ vì ngẫu nhiên!. 

Có bạn sẽ thắc mắc, sao lại phải kết luận trên toàn bộ tập khách hàng cơ chứ! (Mình xin nhắc lại là kết luận trên toàn bộ tập khách hàng, không có nghĩa là bạn cần tính toán trên toàn bộ tập khách hàng). Vậy mình sẽ lấy ví dụ minh họa như sau. 

Bạn chế tạo ra vacxin Prizer để ngừa COVID-19. Và một số người lanh lợi sẽ hỏi rằng, vacxin này có thể giảm tỉ lệ tử vong bao nhiêu cho người được tiêm, những người này có thể là bây giờ, hoặc sau 1 năm nữa. Bạn phải kết luận cho tập người sau 1 năm nữa tiêm vacxin cũng phải có hiệu quả chứ? không lẽ bạn chỉ kết luận cho tập người được tiêm trong năm nay không thôi sao? Nhỡ đâu tập người tiêm 1 năm sau bị biến chứng nặng hơn sau khi tiêm vacxin thì sao? Ai sẽ chịu trách nhiệm cho những người 1 năm sau đó. Đó là lý do tại sao chúng ta cần kết luận trên toàn bộ quần thể (population), toàn bộ tập khách hàng. 

Và để trả lời cho tại sao **Chiến lượt ABTest ngây thơ** là sai thì đó là việc bạn chỉ đơn giản tính các chỉ số trên tập sampling rồi so sánh 2 số đó như là 2 biến bình thường thì bạn chỉ kết luận được trên tập sampling đó mà thôi. Ko trả lời được liệu trên toàn bộ population có tốt hơn thật hay không?


# Các khái niệm về xác suất cơ bản liên quan đến A/B Testing


## Random process
Các bạn có thể suy nghĩ random process là một process là kết quả (outcome) là ngẫu nhiên hay chứ không phải tất định. Một số ví dụ về random process như sau:
- Tung đồng xu
- Tung xúc xắc 
- Lắc một hộp chưa các loại viên bi xanh,đỏ, ... sau đó bốc một viên mà không được nhìn. 
- Sampling ngẫu nhiên tập khách hàng của chúng ta

## event
## Biến ngẫu nhiên (random variable)
Có thể tưởng tượng biễn ngẫu nhiên là biến phụ thuộc vào quá trình ngẫu nhiên. Ví dụ
- Giá trị của mặt xuất hiện trên xúc xắc khi chúng ta tung xúc xắc. 
- Tung 2 con xúc xắc và tính tổng của 2 mặt xuất hiện. 

Đến đây, bạn có thể thấy rằng, giá trị conversion rate của tập khách hàng mà chúng ta sampling là biến ngẫu nhiên.

## Phân bố xác suất (Probability distribution)
Biến ngẫu nhiên nhận một tập giá trị, do đó nó sẽ có cái mà người ta gọi là phân bố xác suất. Cụ thể, phân bố xác suất cho biết khả năng mà biến ngẫu nhiên nhận giá trị đó là bao nhiêu. 

Conversion rate của tập khách hàng mà chúng ta sampling cũng sẽ có phân bố xác suất nhé. 

Phân bố xác suất có rất nhiều loại khác nhau, mỗi loại có vài tham số đặc trưng cho phân bố đó. 

<div class="img-div" markdown="0">
    <img src="/images/abtest/prob_distribution.png" />
    <em>Bức ảnh này thể hiện gia phả của các phân bố xác suất cơ bản</em>
</div>

Mình sẽ giải thích vài loại phân bố xác suất phổ biến cơ bản.
### Phân bố đều (Uniform distribution)
Là phân bố mà xác suất xuất hiện của các giá trị mà biến ngẫu nhận được là bằng nhau 

Mình họa của phân bố đều trong thực tế mà bạn có thể quan sát được là:
- Phân bố xác suất của biễn ngẫu nhiên là giá trị mặt xúc sắc mà bạn quan sát được khi tung con xúc sắc 1 lần. 
- hoặc là 1 con bài mà bạn có thể nhận được trong bộ bài tây 54 lá

<div class='row'>
<span class="col-sm-12 text-center" style="font-size:120%">$$a=softmax(e)$$</span>
</div>

### Phân bố Bernoulli (Bernoulli Distribution)
Là phân bố rời rạc, trong đó, tập giá trị mà biến ngẫu nhiên có thể nhận được chỉ gồm 2 giá trị **Success** họăc **Fail**. Nếu xác suất của success là p thì xác suất của fail là 1 - p. Thông thường, các câu hỏi dạng yes/no (yes/no question) thuộc phân bố  bernoulli. Bernoulli là tên của Jacob Bernoulli người Thụy Sĩ. 

Các ví dụ minh họa biến ngẫu nhiên có phân bố Bernoulli là:
- có xuất hiện mặt ngửa khi tung đồng xu 1 lần hay không?
và 2 ví dụ sau rất phổ biến trong thực tế mà mình gặp phải khi làm việc tại các công ty
- 1 khách hàng có click hay không vào quảng cáo?
- 1 khách hàng có mua sản phẩm hay không? 
- 1 khách hàng có kết bạn khi mình gợi ý hay không?

Nên lưu ý rằng mình ghi số lượng là 1 khách hàng click/mua sắm/kết bạn và 1 lần tung đồng xu. Nếu có 2 khách hàng hoặc 2 lần tung đồng xu thì lúc này biến ngẫu nhiên là trạng thái 2 khách hàng không còn là phân bố bernoulli nữa vì các giá trị mà biến ngẫu nhiên này nhận được là 4 giá trị khác nhau. Lúc này, biến ngẫu nhiên trạng thái 2 khách hàng thuộc phân bố nhị thức ở dưới nhé. 

### Phân bố nhị thức (Binomial distribution)
Mình nghĩ là bắt đầu với một ví dụ để các bạn thấy sự khác biệt do của phân bố nhị thức với phân bố bernoulli mà ta là nói ở trên sẽ đơn giản hơn. 

Chúng ta bắt đầu với phân bố nhị thức bằng cách đặt câu hỏi sau:
- 1 khách hàng có click hay không vào sản phẩm mà chúng ta quảng cáo ?
sau đó chúng ta lại hỏi thêm câu
- nếu bây giờ chúng ta có 10 khách hàng, vậy số khách hàng click vào quảng cáo sẽ như thế nào?

Câu chúng ta vừa hỏi **vậy số khách hàng click vào quảng cáo sẽ như thế nào?** có phân bố nhị thức.

Vậy phân bố nhị thức là phân bố của biến ngẫu nhiên mà khi chúng ta hỏi câu có bao nhiêu lần quan sát được success event trong n lần thực hiện thí nghiệm, mỗi lần thí nghiệm là một câu hỏi yes/no question.

Phân bố nhị thức có mối quan hệ tuyệt vời với phân bố chuẩn mà chúng ta sẽ nói đến ở các phân tiếp theo. 

### Phân bố chuẩn (Normal distribution)
Phân bố này hơi khó để diễn tả bằng lời nói một cách rõ ràng. Tuy nhiên bạn có thể tưởng tượng phân bố chuẩn là phân bố có hình dạng cái chuông (bell curve), đối xứng qua trung tâm. Phân bố này có tính chất rất hay đó là xác suất 1 điểm nằm trong 1std, 2std, 3std (std: là độ lệnh chuẩn) tương ứng là 68%, 95% và 99.8%. Tính chất này gọi là quy tắc 68-95-99.7 . Quy tắc này thường dùng để kiếm tra nhẹ nhàng xem một phân bố dạng bell curve có phải là phân bố chuẩn hay không?  Do đó không phải phân bố nào đối xứng 2 bên và có hình chuông thì cũng là phân bố chuẩn đâu nhé mà ít nhất nó phải thoải mãn tính chất 69-95-99.7

<div class="img-div" markdown="0">
    <img src="/images/abtest/normal_distribution.png" />
    <em>Minh hoạt phân bố chuẩn và quy tắc 68-95-99.7</em>
</div>

Mình nghĩ rằng phân bố chuẩn là phân bố quan trọng nhất trong các loại phân bố bởi vì nó là phân bố phổ biến nhất trong đời sống, lý do phân bố chuẩn lại phổ biến có thể được giải thích bằng central limit theorem mà chúng ta sẽ tìm hiểu ở phần sau nhé. 

Một số biến ngẫu nhiên có phân bố chuẩn là:
- Chiều cao của dân số 
- Chỉ số IQ


### Mối quan hệ giữa binomial distribution và normal distribution
Các bạn hãy nhớ rằng chúng ta có thể dùng phân phối chuẩn để sấp xỉ phân bố nhị thức khi phân bố nhị thức thỏa điều kiện: xác suất p sucess gần 0.5 hoặc số lần thí nghiệm n lớn. 

Khi p càng lệch về 2 phía (xấp xỉ 1 hoặc 0 ) thì càng cần nhiều lần thí nghiệm hơn để phân bố nhị thức xấp xỉ phân bố chuẩn. hoặc đơn giản ghi nhớ công thức np > 10 và n(1-p)>10 

Câu hỏi là tại sao chúng ta lại thích xấp xỉ phần bố nhị thức thành phân bố chuẩn làm gì ? bởi vì phân bố chuẩn được sử dụng rộng rãi, chúng ta dễ dàng ghi nhớ các tính chất của nó, cũng như z score và p value, nên chúng ta dễ tính nhẩm hơn. Ngày nay, chúng ta sài máy tính phổ biến nên việc tính nhẩm này cũng không cần thiết lắm. 

# Các điều quan trọng khi làm ABTest 
AB testing chia ngẫu nhiên user thành 2 tập có kích thước tương đương nhau:
- control: tập mặc định trước khi áp dụng mô hình cải tiến
- variant: tập áp dụng mô hình cải tiến. 
 
Sử dụng kiểm định giả thuyết để  so sánh chỉ số cần quan tâm trên 2 tập trên, và từ đó quyết định xem có nên áp dụng mô hình cải tiện hay không?

Có 3 điểm cần lưu ý khi làm ABTest. 
- Cần chia user thành 2 tập ngẫu nhiên. Việc chia ngẫu nhiên làm cho các tác động khác (ngoại trừ cải tiến mà chúng ta định áp dụng cho tâp control) phân bố đều vô 2 tập. Từ đó đảm bảo không có selection bias khi chúng ta thực hiện thí nghiệm. 
- 2 tập user phải có kích thước tương đương. Để thấy tại sao lại cần chia bằng nhau mình sẽ đưa ra một ví dụ trong trường hợp cực đoạn như sau. Chúng ta có tập control gồm 2 khách hàng, và tập variant gồm 100k khách hàng. để đạt được tỉ lệ mua hàng là 50% thì tập control sẽ dễ dàng có cơ hội hơn nhiều so với tập variant. Thực tế mình gặp một số trường hợp cho rằng có thể chia user thành 2 tập có kích thước không tương nhau vì cuối cùng tỉ lệ mua hàng đều được chuẩn hóa bởi mẫu số nên có thể so sánh 2 tỉ lệ với nhau. nhưng mà suy nghĩ này đang ngây thơ mà thôi. 
- và điều quan trọng nhất đó là so sánh các chỉ số bằng <span style="color:red">*kiểm định giả thuyết*</span>. Do chúng ta so sánh các chỉ số có tính ngẫu nhiên như phân tích ở trên, nên không tể nào so sánh kiểu tuyệt đối như 2 > 1... mà chúng ta phải so sánh phân bố của các giá trị mà biến ngẫu nhiên có thể nhận được. 

Có nhiều tài liệu tham khảo trên internet nói về ABtest nhưng họ lại không đề cập việc phải dùng kiểm định giả thuyết để so sánh các chỉ số do đó thường gây ra hiểu lầm cho rất nhiều người. Ở phần tiếp theo, mình sẽ trình bày chi tiết cách sử dụng kiểm định giả thuyết trong ABTest như thế nào.

# Chúng ta có thể so sánh 2 biến ngẫu nhiên như thế ?
Khi 2 biến ngẫu nhiên có cùng ý nghĩa, chúng ta có thể dùng ý tướng sau đây để so sánh 2 phần bố của chúng để từ đó kết luận biến x có lớn hơn y hay không?. Cụ thể hơn là 
- Chúng ta có thể nói x > y khi 95% các giá trị mà x nhận được lớn hơn tất cả 95% các giá trị mà y nhận được. 
- Chúng ta có thể nói x < y khi mà 95% các giá trị x nhận được bé hơn tất cả 95% các giá trị y nhận được.
- Chúng ta có thể nói x = y khi mà 95% các giá trị x nhận được bằng với một phần tử nào đó trong tập 95% các giá trị của y
- Chúng ta có thể nói x!= y khi mà 95% các giá trị x nhận được không bằng với bất kì phần tử trong tập 95% các giá trị của y
Để dễ hiểu hơn ý tưởng này, các bạn hãy xem minh họa dưới đây.
(minh họa)

# Cách tính conversion rate chính xác. 
Có thể chúng ta đã hiểu được cần chia tập khách hàng thành 2 tập a,b ngẫu nhiên bằng nhau, chúng ta cũng hiểu cần so sánh 2 biến ngẫu nhiên dựa vào phân bố của chúng chứ không phải so sánh giá trị tuyệt đối. Nhưng mà đến lúc này, chúng ta vẫn có thể vẫn mắc một sai lầm chết người nữa là **chúng ta lại đi cân đo đong đếm 2 biến ngẫu nhiên mà kết quả việc này lại không trả lời chính xác được câu hỏi của business**. Lý dó là thứ nhất câu hỏi của business lỏng lẻo, thứ 2 là chúng ta tính sai hay ngu ấy. 

## Chọn success event 

# Cách AB test trong thực tế hoạt động
## Central Limit Theorem
## ABtest for propotion
## ABtest for mean

# ABTest khi không biết phân bố của biến ngẫu nhiên là 

# 2 tail-test, one-tail test
# Một số quan điểm sai lầm
3) Saying “We accept the Null hypothesis”
You either reject the null hypothesis or fail to reject the null hypothesis.




# Tham khảo
- https://math.stackexchange.com/questions/845769/why-does-the-normalized-z-score-introduce-a-square-root-and-some-more-confusio
