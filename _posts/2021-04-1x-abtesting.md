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
## event
## Random variable

## Probability distribution

Như mình đã nói ở trên, chúng ta cần công cụ để xem xét các sự kiện ngẫu nhiên, và hàm phân bố xác suất là một trong những khái niệm cơ bản nhất mà chúng ta cần biết. 

Hàm phân bố xác suất của biến ngẫu nhiên cho chúng ta biết các giá trị có thể có và xác suất tương ứng giá trị đó mà biến ngẫu nhiên có thể nhận đươc. 

# Trực quan cách A/B testing hoạt động 
# Cách tính conversion rate chính xác. 
## Chọn success event 

# Cách AB test trong thực tế hoạt động
## Central Limit Theorem
## ABtest for propotion
## ABtest for mean

# 2 tail-test, one-tail test

# Tham khảo
- https://math.stackexchange.com/questions/845769/why-does-the-normalized-z-score-introduce-a-square-root-and-some-more-confusio
