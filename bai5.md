# Bài 5: Cơ chế In-Context Learning trong Phân loại Review Khách hàng
Phần 1: Phân tích cơ chế In-Context Learning (ICL)Bản chất không cập nhật trọng số: In-Context Learning cho phép mô hình ngôn ngữ lớn (LLM) hiểu và thực hiện nhiệm vụ mới ngay trong không gian ngữ cảnh (prompt) mà không cần qua quá trình huấn luyện lại (retraining) hay cập nhật trọng số (weights/parameters).   Cơ chế hoạt động: Khi cung cấp các ví dụ mẫu (đầu vào kèm đầu ra mong muốn), mô hình tận dụng khả năng nhận diện mẫu (pattern recognition) và cơ chế tự chú ý (Self-Attention). Các ví dụ này hoạt động như một "khuôn mẫu" định hướng không gian tiềm ẩn (latent space), giúp kích hoạt các tri thức đã học sẵn để suy luận ra logic xử lý cho trường hợp tiếp theo.Khắc phục bẫy ngữ nghĩa: Zero-shot chỉ dựa trên nghĩa hiển ngôn (Surface Lexical Meaning) nên dễ bị đánh lừa bởi từ ngữ tích cực đơn lẻ (ví dụ: đẹp mê ly, khỏi cần bật lò sưởi). ICL giúp mô hình đọc hiểu ngữ cảnh toàn diện, nhận diện các lớp lang ẩn ý, mỉa mai (sarcasm) thông qua việc so sánh tương quan giữa các yếu tố kỹ thuật nguy hiểm (như nhiệt độ 98°C) với lời khen hình thức.   Phần 2: Thiết kế Prompt Few-shot hoàn chỉnhDưới đây là cấu trúc Prompt Few-shot mẫu để hướng dẫn AI phân tích chính xác các sắc thái mỉa mai, khen đểu tại TechShop:PlaintextBạn là một chuyên gia phân tích chất lượng sản phẩm công nghệ. Nhiệm vụ của bạn là đọc kỹ các đánh giá từ khách hàng, nhận diện chính xác sắc thái mỉa mai (khen đểu) để phân loại cảm xúc thực tế là TÍCH CỰC hay TIÊU CỰC.

Dưới đây là các ví dụ mẫu:

### Ví dụ 1 (Vấn đề về Nhiệt độ):
- Input: "Máy thiết kế đẹp mê ly, cầm rất đầm tay, mùa đông ôm máy chơi game thì khỏi cần bật lò sưởi vì nhiệt độ CPU lên hẳn 98 độ C!"
- Output: { "Sentiment": "TIÊU CỰC", "Reason": "Sử dụng từ ngữ khen hình thức ở đầu nhưng bản chất là chê thiết bị quá nóng (98°C), gây mất an toàn khi sử dụng." }

### Ví dụ 2 (Vấn đề về Pin):
- Input: "Điện thoại pin trâu kinh khủng, sáng sạc đầy rút ra, đến trưa lướt web chút xíu là tụt còn 5% pin, phải công nhận là máy biết cách thử thách lòng kiên nhẫn ghê!"
- Output: { "Sentiment": "TIÊU CỰC", "Reason": "Dùng từ 'pin trâu' mỉa mai thực tế pin sụt cực kỳ nhanh (còn 5% vào buổi trưa)." }

### Ví dụ 3 (Vấn đề về Giao hàng):
- Input: "Shop giao hàng nhanh như vận tốc ánh sáng, đặt hàng từ năm ngoái mà năm nay mới thấy shipper gọi điện nhận hàng, tuyệt vời ông mặt trời!"
- Output: { "Sentiment": "TIÊU CỰC", "Reason": "Dùng từ ngữ phóng đại 'nhanh như vận tốc ánh sáng' để mỉa mai thời gian giao hàng cực kỳ chậm chạp." }

---
### Dữ liệu cần phân tích:
- Input: "[Nhập review mới của khách hàng vào đây]"
- Output: 