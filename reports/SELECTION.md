# Vì sao chọn lô này?

Nếu chỉ được sửa 5 ảnh, tôi chọn `frame_0182.jpg` (hạng 1, điểm 0.9591, 72.8 giây), `frame_0369.jpg` (hạng 2, điểm 0.9324, 147.6 giây), `frame_0380.jpg` (hạng 3, điểm 0.9170, 152.0 giây), `frame_0326.jpg` (hạng 4, điểm 0.9155, 130.4 giây) và `frame_0331.jpg` (hạng 5, điểm 0.9154, 132.4 giây). Đây là năm ảnh đứng đầu danh sách theo điểm chọn. `frame_0369.jpg` và `frame_0380.jpg` cách nhau 4.4 giây, nên vẫn có khả năng gần giống cảnh; nếu ngân sách rất hạn chế, tôi sẽ ưu tiên `frame_0369.jpg` và dùng ảnh ở thời điểm khác để tăng độ đa dạng.

Trong 12 ảnh AI đã chọn, tôi tập trung xem `frame_0182.jpg`, `frame_0099.jpg` và `frame_0107.jpg`. Ba ảnh có điểm cao, nhiều xe và nhiều box không chắc chắn: lần lượt có 18, 14 và 15 box ambiguous. Đây là những ảnh có khả năng chứa cả xe bị bỏ sót, xe bị che khuất hoặc các box cần chỉnh ranh giới.

`frame_0372.jpg` có điểm 0.9101, đứng hạng 6, cao hơn nhiều ảnh được chọn, nhưng không nằm trong lô 12 vì cách `frame_0369.jpg` chỉ 1.2 giây. Hai ảnh gần nhau có thể mô tả gần như cùng một cảnh, nên chọn cả hai sẽ tốn công mà cung cấp ít thông tin mới hơn. Đây là tác dụng của điều kiện khoảng cách thời gian tối thiểu.

Điểm cao cho biết model đang bất định hoặc ảnh có nhiều trường hợp khó, chứ không chứng minh rằng sửa ảnh đó chắc chắn sẽ làm model tốt hơn. Kết quả còn phụ thuộc chất lượng box tôi sửa và mức độ đại diện của 12 ảnh đối với các ảnh còn lại.
