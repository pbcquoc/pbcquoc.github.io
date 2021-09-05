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

# Phương pháp kiểm tra đơn giản và không chính xác.
Để giải quyết câu hỏi liệu tính năng mới có thực sự tốt hơn hay không? Chúng ta thường nghĩ đến phương pháp kiểm tra sai lầm như sau: Cứ mở tính năng mới cho tất cả khách hàng sử dụng, sau đó quan sát xem liệu các chỉ số như số khách sử dụng hằng ngày có tăng hay không, (và các chỉ số quan trọng khác tùy mô hình kinh doanh). Nếu thấy tăng thì chúng ta bảo là tốt hơn, và cứ thế tự tin rằng mình đang làm điều tốt đẹp. 

Phương pháp này tuy sai, nhưng lại đơn giản nên hầu như ai cũng nghĩ được và vì thế nên lại thường được sử dụng. Nhưng mình nhấn mạnh lại một lần nữa là phương pháp này sai vì những vấn đề không được giải quyết sau:
- Trong thời gian bạn chạy tính năng mới, nhưng yếu tố khác tác động vào làm tăng chỉ số mà bạn đang đo. Ví dụ như bạn đang kiểm tra xem giao diện mới có làm tăng lượt click vào sản phẩm so với giao diện cũ hay không thì bộ phận khác của công ty tăng tiền quảng cáo, tạo ra các mã giảm giá, khuyến mãi cho khách hàng thì có thể chỉ số bạn đang theo dõi cũng tăng theo mặc dù cái giao diện mới chả tạo ra thêm lượt click so với cái giao diện cũ. 

# Xác suất cơ bản liên quan đến A/B Testing

## Random process
## Random event
## Random variable

## Probability distribution

Như mình đã nói ở trên, chúng ta cần công cụ để xem xét các sự kiện ngẫu nhiên, và hàm phân bố xác suất là một trong những khái niệm cơ bản nhất mà chúng ta cần biết. 

Hàm phân bố xác suất của biến ngẫu nhiên cho chúng ta biết các giá trị có thể có và xác suất tương ứng giá trị đó mà biến ngẫu nhiên có thể nhận đươc. 

# Cách A/B testing hoạt động 
# Cách tính conversion rate chính xác. 
## Chọn success event 

# 2 tail-test, one-tail test
