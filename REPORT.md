- Mã học viên theo lớp: 2A202602317
- Ngày / CVAT local: 17/09/2026 / http://localhost:8080
- Công cụ đã dùng: Brush, Polygon, OpenCV (Intelligent Scissors), Eraser.

## 1. Bài đã nộp

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | easy_semantic.zip | 3 / 3 | 20 |
| medium_instance | medium_instance.zip | 3 / 3 | 32 |
| hard_panoptic | hard_panoptic.zip | 2 / 2 | 30 |
| cp1_holes | cp1_holes.zip | 1 / 1 | 3 |
| cp2_slice | cp2_slice.zip | 1 / 1 | 3 |
| cp5_occlusion | cp5_occlusion.zip | 1 / 1 | 3 |
| cp3_thin | cp3_thin.zip | 1 / 1 | 3 |
| cp4_curb | cp4_curb.zip | 1 / 1 | 3 |
| cp6_coverage | cp6_coverage.zip | 1 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

## 2. Một quyết định trước khi dùng gợi ý

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: Ảnh 00000031542.jpg, chiếc xe bán tải (truck) ở nửa trên bên phải bị người phụ nữ áo dài che khuất.
- Class và quy tắc tôi dùng để chọn biên: Class `truck`. Quy tắc: "Chỉ vẽ phần nhìn thấy", biên mask của xe dừng chính xác tại mép người phụ nữ áo dài, không đoán viền bị che khuất.
- Nếu dùng gợi ý sau đó: Tôi có thử dùng công cụ OpenCV của CVAT nhưng vùng gợi ý bị lẹm viền xe lên người phụ nữ (bắt nhầm nét do cảnh rối). Hành động sửa: Tôi đã chuyển sang dùng công cụ Brush/Polygon để vẽ tay lại ranh giới, ép viền mask bám sát mép người ở tiền cảnh.
- Nếu không dùng gợi ý: (Đã mô tả ở trên).

## 3. Một lỗi tôi tìm thấy và sửa

- Task/ảnh/vùng: Task `cp1_holes` / ranh giới giữa chiếc xe máy và chiếc xe rơ-moóc caravan phía sau.
- Lỗi thuộc loại: biên (lẹm viền vật thể).
- Bằng chứng tôi nhìn thấy: Khi tôi tạo Object `CAR 5` cho chiếc caravan màu trắng phía sau, nét vẽ ban đầu vô tình lấn đè lên phần kính chắn gió và phần yên của chiếc xe máy Honda phía trước.
- Quy tắc và hành động sửa: Theo quy tắc "chỉ vẽ phần nhìn thấy", vật hậu cảnh không được đâm xuyên qua vật tiền cảnh. Tôi đã dùng Polygon/Eraser gọt lại cẩn thận mép viền màu xanh của `CAR 5`, ép nó dừng lại chính xác tại viền xe máy, đảm bảo 2 instance tách bạch hoàn toàn.
- Sau sửa đã Save và export lại chưa? Đã Save và export lại file `cp1_holes.zip` định dạng COCO 1.0 (kết quả điểm tự đánh giá: chưa có).

## 4. Ba ca chưa chắc hoặc đã cân nhắc

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| 1. `cp1_holes` (Chiếc rơ-moóc caravan phía sau) | 1 là bỏ qua không gán nhãn vì nó là xe kéo rơ-moóc, 2 là gom nó vào nhóm xe (`car`). | Dù bộ nhãn COCO không có class "caravan", nhưng nó có cấu trúc của một phương tiện giao thông cỡ lớn chiếm diện tích hậu cảnh. | Tôi quyết định vẽ mask và gán class `car` (ID: `CAR 5`) cho chiếc caravan để ảnh không bị bỏ sót vật thể lớn. |
| 2. `cp4_curb` (Ranh giới vỉa hè chỗ có túi rác) | 1 là cắt ranh giới theo vệt rác/đất vương vãi, 2 là cắt chuẩn theo gờ bê tông bó vỉa. | Ranh giới phân chia được xác định theo chức năng vật lý (gờ bó vỉa) chứ không phân chia theo màu sắc rác bẩn. | Cắt ranh phân tách cực kỳ rõ ràng vùng màu tím (`road 3`) và vùng màu hồng (`sidewalk 6`) bám sát đúng gờ bê tông. |
| 3. `cp3_thin` (Biển báo Cross Island Pkwy) | 1 là vẽ mask mập mạp trùm luôn cả cụm biển, 2 là phải zoom lên tô kỹ tách riêng biển và thân cột. | Quy tắc đối với các nét mảnh là không được để mask ăn lẹm ra nền trời (`sky`). | Tôi dùng Polygon viền kỹ, tách bạch rõ mảng `traffic sign` (màu vàng) và cấu trúc thép `pole` khỏi nền trời. |