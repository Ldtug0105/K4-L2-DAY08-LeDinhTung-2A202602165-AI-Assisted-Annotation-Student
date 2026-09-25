# Báo cáo Lab Ngày 08: Học chủ động cho bộ phát hiện xe

Họ và tên: Lê Đình Tùng

Công cụ gán nhãn đã dùng: CVAT

## 1. Dữ liệu và cách chia tập

Tập chưa gán nhãn (pool) và tập kiểm thử (test set) được chia theo trục thời gian, có vùng đệm ở giữa, để hạn chế việc các khung hình gần như giống nhau xuất hiện ở cả hai tập. Camera đứng một chỗ và một chiếc xe có thể nằm trong hình vài giây. Nếu chia ngẫu nhiên, cùng một xe hoặc cùng một cảnh có thể vừa được dùng để chọn/gán nhãn vừa được dùng để chấm điểm. Khi đó số đo trên tập test có thể cao hơn khả năng tổng quát thật của model.

## 2. Mô hình khởi đầu lạnh (cold start)

Vòng 0 dùng `yolov8n cold start (COCO car+bus+truck)`, không có ảnh train và không có box train. Trên 20 ảnh test, model đạt AP50 0.771, P@0.25 là 0.925, R@0.25 là 0.489 và F1 là 0.640. Recall theo kích thước là 0.182 với xe nhỏ, 0.547 với xe vừa và 0.561 với xe lớn. Như vậy model bỏ sót xe nhỏ nhiều hơn rõ rệt; độ chính xác cao nhưng độ phủ còn thấp.

Trong `outputs/compare_round0.jpg`, các box cần chú ý là những xe xa, tối hoặc bị che vì model có thể bỏ sót hoặc đặt box lệch. Tuy nhiên nhãn dùng để chấm cũng do máy tạo và chưa được người kiểm từng box, nên một trường hợp model không khớp nhãn chưa chắc hoàn toàn là lỗi của model. Cần đối chiếu ảnh gốc và guideline trước khi kết luận.

## 3. Chiến lược chọn mẫu

Điểm chọn dùng công thức `score = W_U*U + W_A*A + W_D*D`. Thành phần `U` đo độ bất định của model, `A` phản ánh số lượng hoặc mức độ các box còn lưỡng lự, còn `D` ưu tiên ảnh khác thời gian với những ảnh đã chọn. Điều kiện `MIN_GAP_S` yêu cầu hai ảnh được chọn cách nhau ít nhất 2 giây để tránh chọn nhiều khung gần như trùng cảnh.

Ba frame thuộc lô 12 ảnh được chọn là `frame_0182.jpg` (điểm 0.9591, hạng 1), `frame_0099.jpg` (0.9063, hạng 8) và `frame_0107.jpg` (0.8876, hạng 14). Chúng có nhiều box ambiguous và cần người kiểm tra. `frame_0372.jpg` có điểm 0.9101, hạng 6, nhưng không được chọn vì cách `frame_0369.jpg` chỉ 1.2 giây; chọn cả hai sẽ ít đa dạng hơn. Điểm bất định không chứng minh ảnh đó chắc chắn cải thiện model, vì hiệu quả còn phụ thuộc việc sửa nhãn và mức độ đại diện của ảnh.

## 4. Các vòng học chủ động (active learning)

Vòng 0 có 0 ảnh train và 0 box train, AP50 là 0.771. Ở vòng 1, tôi đã sửa 12 ảnh: giữ nguyên 142 box, chỉnh 13 box, xoá 14 box và thêm 188 box; tổng số box sau sửa là 343. Báo cáo `outputs/round1_diff.md` cho thấy pre-label ban đầu có 169 box, nên model đã bỏ sót nhiều xe trong lô được chọn.

Kết quả AP50, `compare_round1.jpg` và `metrics_round1.json` chỉ có sau khi tải `day8_data.zip` mới lên Colab và chạy notebook vòng 1. Vì vòng 1 chưa được chạy lại trong Colab tại thời điểm viết báo cáo, chưa thể kết luận AP50 tăng hay giảm hoặc mô tả một thay đổi của model sau fine-tune mà không bịa số liệu. Sau khi Colab tạo kết quả, cần chép dòng vòng 1 vào `reports/rounds_table.md`, so sánh với AP50 0.771 và đối chiếu `compare_round0.jpg` với `compare_round1.jpg`.

`BLIND_SCAN.md` là quan sát độc lập trước khi xem pre-label; `REVIEW_LOG.csv` ghi các box tôi đã giữ, chỉnh, xoá hoặc thêm trên CVAT; còn kết quả sau train sẽ là dự đoán của model mới. Một ca khó theo guideline là xe bị che hoặc bị cắt ở mép ảnh: chỉ vẽ phần thân xe nhìn thấy, không gộp hai xe sát nhau thành một box.

## 5. Kết luận và giới hạn

Vòng 1 đã tạo ra một tập sửa có nhiều box bổ sung, đặc biệt là 188 box bị pre-label bỏ sót. Tôi chưa quyết định dừng hay tiếp tục dựa trên AP50 vì model vòng 1 chưa chạy xong. Quyết định tiếp theo cần dựa trên AP50 cùng ảnh so sánh, không chỉ dựa vào số box đã sửa.

Hai nhóm còn yếu cần theo dõi là xe rất xa chỉ còn hai chấm đèn và xe bị che hoặc bị cắt ở mép ảnh. Rà thêm các ảnh tương tự sẽ tốn thời gian, đồng thời không nên chọn hai ảnh sát nhau vì chúng gần như cùng một cảnh. Tập kiểm thử chỉ có 20 ảnh, các xe quá nhỏ có thể bị bỏ qua khi chấm, và nhãn tham chiếu do model tạo chưa được người kiểm thủ công. Vì vậy AP50 chỉ là bằng chứng giới hạn. Nếu AP50 giảm sau vòng 1, tôi sẽ kiểm tra lại các box đã thêm, chỉnh, xoá trong `round1_diff.md` và đối chiếu ảnh trước khi train thêm.
