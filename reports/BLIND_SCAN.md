# Quét độc lập trước khi xem pre-label

Frame: `to_label\round1\images\train\frame_0099.jpg`

Số xe nhìn thấy bằng mắt: 26

Hai vị trí dễ bị AI bỏ sót hoặc vẽ sai, kèm mô tả xe: 
- bị giới hạn bởi khung hình chỉ hiện ra một chút phần đầu hoặc đuôi xe
- bị vật thể khác che mất
- 2 xe chồng lên nhau chỉ hở một chút thì mắt thường có thể phân biệt được nhưng AI khó có thể phân biệt đc

Chạy `python3 tools/lock_blind.py` ngay sau khi điền. Sau đó giữ file này nguyên vẹn.

