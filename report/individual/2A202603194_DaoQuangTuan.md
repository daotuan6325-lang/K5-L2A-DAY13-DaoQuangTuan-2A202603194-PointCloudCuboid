# Báo cáo Day 13 — Đào Quang Tuấn, 2A202603194

> **Cách dùng mẫu này:** đổi tên file thành `<MSSV>_<HoTen>.md` (họ tên không dấu, viết liền), ví dụ `2A202600123_NguyenVanAn.md`. Đổi tên, đừng chép thành file thứ hai.
>
> Xoá mọi dấu TODO và dòng gợi ý (dòng bắt đầu bằng `>`). Mỗi con số lấy từ **bảng A** hoặc **phiếu tự kiểm** trong `submission/`, kèm đơn vị (m, °). Ảnh để trong `report/individual/images/` (nộp LMS trong file zip, không commit); trong báo cáo chỉ ghi **tên file ảnh**. Báo cáo này nằm trên repo public: không chép biển số, chữ trên biển hiệu hay biển chỉ đường, tên đường, địa danh; nhắc tới bạn khác thì ghi MSSV, không ghi họ tên. Cả báo cáo ~2 trang.

## 1. Kết luận trước
TODO(học viên): mã số, task `D13-…`, 10 frame của bạn; hệ điều hành, trình duyệt, phiên bản CVAT (tên tài khoản → About); MSSV bạn ngồi cạnh. Rồi 2–3 câu: tổng bao nhiêu cuboid, frame nào ít nhất và vì sao; `selfcheck` cờ bao nhiêu cuboid trên tổng số; bạn tin phần nào nhất và phần nào còn yếu nhất (hướng, đáy, kích thước, class, vật xa).

TODO(học viên): cách dùng CVAT, 3 dòng: đổi góc nhìn (nhìn từ trên xuống, xoay, dời); vẽ và chỉnh một cuboid (Shape, nháy đúp, Top/Side/Front); bật mũi tên hướng (Cuboid orientation) và xem cuboid trên ảnh camera (`preview`).

## 2. Đọc frame (Bước 2)
Hai frame đọc kỹ. Ảnh: `t1-<frame>-3d`, `t1-<frame>-top` (ảnh nhìn từ trên xuống, có đánh dấu phía trước, +x, +y).

| Frame | Phía trước xe ở cạnh nào của ảnh nhìn từ trên xuống; +x, +y chỉ về đâu | Mặt đường (phẳng/dốc; thấp hơn LiDAR ~ m) | Vật xa nhất đọc được (~ m) | Loại vật thấy | Vật phía sau / hai bên | Cụm chưa đọc được |
|---|---|---|---|---|---|---|
| s01f03 | Phía trước (−y) ở cạnh trên ảnh top toàn cảnh; +x chỉ sang trái ảnh (bên trái xe); +y chỉ xuống cạnh dưới (sau xe) | Phẳng, thấp hơn LiDAR khoảng 1,7 m | Khoảng 45–50 m (xe con rìa trên-trái, 2 xe con rìa dưới-trái và xe lớn ở góc dưới-phải) | 16 xe con, 1 xe tải vừa, 1 xe lớn (xe buýt hoặc xe tải thùng dài), 1 người hoặc xe hai bánh; xung quanh có cây cối và tường/công trình hai bên | Sát phải 1 xe con (~3–4 m); trước-trái 1 xe con (~10 m), sau-trái 1 xe con (~8 m); bên trái 6 xe con và 1 xe tải (~12–20 m); phía sau 2 xe con (~12–18 m), 1 cụm nhỏ sau-phải (~20 m), 1 xe con sau-phải (~28 m), 1 xe lớn góc dưới-phải (~45 m) | TODO(học viên) |
| TODO(học viên) s01f24 | | | | | | |

TODO(học viên): 3 điều **chỉ camera** cho biết và 3 điều **chỉ LiDAR** cho biết, lấy từ chính hai frame trên.

Tám frame lướt:

| Frame | Phía trước ở cạnh nào của ảnh nhìn từ trên xuống | Điều lạ (nếu có) |
|---|---|---|
| TODO(học viên) | | |

## 3. Ba cuboid có hướng dẫn (Bước 3)
Ảnh: `t2-a-car`, `t2-b-bus-truck`, `t2-c-ped-cyclist`.

| Vật | Frame, idx | Class | Mũi tên yaw đúng hướng? | Đáy chạm đường? | Đã kiểm Top/Side/Front + ảnh? | Kích thước trong bảng 4.3? |
|---|---|---|---|---|---|---|
| (a) xe con gần | TODO(học viên) | | | | | |
| (b) xe buýt hoặc xe tải | TODO(học viên) | | | | | |
| (c) người đi bộ hoặc xe hai bánh | TODO(học viên) | | | | | |

TODO(học viên): vòng hiệu chuẩn: cuboid của bạn lệch so với các bạn ở điểm nào, cả lớp chốt cách hiểu guideline nào? Và các vật bạn **bỏ qua vì không đủ cơ sở** (frame, vị trí, mô tả, loại nghi vấn như UNCERTAIN_CLASS / LOW_VISIBILITY), hoặc ghi "không có".

## 4. Bốn hiện tượng chất lượng (Bước 4)
Nguyên nhân ghi là **giả thuyết**. Frame mượn thì ghi rõ nguồn: task chung, hoặc MSSV của bạn cho mượn.

| Hiện tượng | Frame / tên ảnh / nguồn | Nguyên nhân (giả thuyết) | Thấy gì trên point cloud | Hệ quả khi gán (theo guideline) |
|---|---|---|---|---|
| 1. Bóng che | TODO(học viên) | | | |
| 2. Thưa ở xa | TODO(học viên) | | | |
| 3. Phản xạ thấp / ít điểm bất thường | TODO(học viên) | | | |
| 4. Điểm lạ | TODO(học viên) | | | |

## 5. Số từ cuboid của tôi (Bước 5)
Lấy từ `submission/D13-<MÃ SỐ>-bangA.csv`; số điểm lấy từ cột `points_in` của `-tukiem.csv`. Mặt đường khoảng −1,7 m: z đáy lệch hơn 0,3 m là lơ lửng hoặc chìm.

| Cuboid (frame, idx, class) | z | h | z đáy = z − h/2 | dist_m | r = √(x²+y²+z²) | yaw_deg | Nhận xét (lơ lửng / chìm / chạm đường) |
|---|---|---|---|---|---|---|---|
| TODO(học viên) | | | | | | | |
| TODO(học viên) | | | | | | | |
| TODO(học viên) | | | | | | | |

TODO(học viên): (a) cuboid **gần nhất** và **xa nhất**: `dist_m` và r của từng cái. (b) Kích thước l, w, h của một người đi bộ (hoặc xe hai bánh) và một xe con so với bảng 4.3: chiều nào ngoài khoảng, vì sao. (c) `yaw_deg` của một xe cùng chiều phía trước và một xe ngược chiều (nếu có): có gần 270° và 90° không.

Mật độ theo khoảng cách (4 cuboid ở các `dist_m` khác nhau):

| Cuboid (frame, idx, class) | dist_m | Số điểm (`points_in`) |
|---|---|---|
| TODO(học viên) | | |

TODO(học viên): một câu quy luật ("xa gấp … lần thì số điểm còn khoảng …") và so với đường cong ở slide bài giảng (xe con: khoảng 1968 điểm ở 0–10 m, 718 ở 10–20 m, 214 ở 20–30 m, 72 ở 30–40 m, 19 ở 60–80 m).

**Tự kiểm.** Lần `sync` đầu tiên: `selfcheck` cờ 17/144 cuboid (phiếu `submission/D13-2A202603194-tukiem.csv`). Cảnh báo chỉ là gợi ý xem lại; với mỗi hộp tôi mở lại trên CVAT và ảnh `preview`, rồi sửa hoặc giữ nguyên kèm lý do.

Cột "Hộp" ghi `idx` ở lần 1; sau khi xoá hộp, `idx` lần 2 đổi như ghi trong ngoặc.

| Hộp lần 1 (frame #idx, class) | Cảnh báo của `selfcheck` (lần 1) | Xử lý | Kết quả `sync` lần 2 |
|---|---|---|---|
| s01f03 #9, #10 Pedestrian | Hai hộp chồng lên nhau; #9 có 53 điểm sát ngoài mép | **Đã sửa:** một người bị vẽ hai hộp, xoá một hộp, giữ hộp còn lại | Còn 1 hộp (#9), hết cờ |
| s01f24 #3 Pedestrian | Rất ít điểm (3), cách 38,86 m | **Đã xoá:** 3 điểm không đủ cơ sở (LOW_VISIBILITY) | Không còn trong bảng A |
| s05f07 #1 Pedestrian | Rất ít điểm (4), cách 46,83 m | **Đã xoá:** 4 điểm, sát ranh 50 m, không đủ cơ sở (LOW_VISIBILITY) | Không còn trong bảng A |
| s01f03 #2 Bus | 102 điểm sát ngoài mép; width 3,11 m | **Đã sửa:** thu width 3,11 m → 2,50 m (đúng bảng 4.3 xe buýt 2,50–2,60 m), dời tâm cho khớp thân xe | Hết cờ |
| s05f42 #6 Pedestrian | Đáy cao hơn mặt đường 0,52 m; 43 điểm sát ngoài mép | **Đã sửa:** hạ đáy z_bottom −0,605 m → −0,937 m | Khoảng hở với mặt đường còn 0,19 m, hết cờ |
| s05f42 #8 Car | width 2,36 m, height 2,50 m ngoài khoảng của Car | **Đã sửa:** đổi class Car → Truck (quá cao, rộng so với xe con) | Hết cờ |
| s03f12 #3 Pedestrian | Đáy cao hơn mặt đường 0,78 m; 15 điểm sát ngoài mép | **Đã sửa:** hạ đáy qua hai lần, z_bottom −0,770 m → −1,230 m | Hết cờ lơ lửng; còn cờ 15 điểm sát ngoài mép (giữ nguyên) |
| s05f07 #4 Car (sau: #3) | Đáy thấp hơn mặt đường 1,00 m (chìm?) | **Giữ nguyên** sau khi thử chỉnh (z_bottom −1,515 m → −1,651 m → −1,530 m): nâng hộp lên khoảng 1 m thì đáy không còn khớp điểm LiDAR của xe; giả thuyết là `selfcheck` ước mặt đường quanh hộp lệch (lấy nhầm điểm vật khác quanh xe ở ~37 m) | Còn cờ chìm 1,02 m |
| s01f03 #14 Car (sau: #13) | Đáy cao hơn mặt đường 0,49 m (lơ lửng?) | **Đã sửa:** hạ đáy z_bottom −1,405 m → −1,883 m | Hết cờ |
| s05f42 #2 Car | width 2,31 m, height 2,42 m ngoài khoảng của Car | Chưa sửa | Vẫn cờ kích thước |
| s01f24 #16 Pedestrian (lần 2: #15) | 157 điểm sát ngoài mép | **Giữ nguyên:** cờ "điểm sát ngoài mép" gặp cả ở nhãn chuẩn với người, xe hai bánh (22–28%, theo `docs/SELF_CHECK.md`) | Còn cờ |
| s02f23 #6 Pedestrian | 11 điểm sát ngoài mép | **Giữ nguyên:** như trên | Còn cờ |
| s02f23 #8 Pedestrian | 30 điểm sát ngoài mép | **Giữ nguyên:** như trên | Còn cờ |
| s02f23 #11 Cyclist | 50 điểm sát ngoài mép | **Giữ nguyên:** như trên | Còn cờ |
| s03f12 #10 Pedestrian | 25 điểm sát ngoài mép | **Giữ nguyên:** như trên | Còn cờ |
| s03f44 #2 Pedestrian | 20 điểm sát ngoài mép | **Giữ nguyên:** như trên | Còn cờ |

Sau khi sửa và chạy lại `sync` (lần cuối): bảng A còn **141 cuboid** (xoá 3 hộp: 1 hộp trùng, 2 hộp quá ít điểm), `selfcheck` còn cờ **9/141** cuboid. Hai cuboid sửa rõ nhất: s01f03 #2 Bus (width 3,11 m → 2,50 m) và s01f03 #14 Car (z_bottom −1,405 m → −1,883 m, hết lơ lửng).

TODO(học viên): đổi kiểm: MSSV người đã kiểm chéo với bạn, hai con số nào được đối chiếu với bảng A, khớp hay lệch (lệch thì đã sửa gì).

## 6. Ý nghĩa của nhãn, cách dùng AI và điều còn chưa chắc
TODO(học viên): (a) hai câu, nói từ phía model sẽ học trên nhãn này: vì sao nhãn 3D đo bằng mét chứ không phải pixel; vì sao vật thưa điểm cần quy tắc riêng. (b) Dùng AI? Ghi 3 dòng: công cụ, câu hỏi chính, bạn dùng kết quả thế nào. Không dùng: ghi "không dùng". (c) Điều còn chưa chắc, lỗi phát hiện sau hạn (không chấm; viết bằng lời, không chép số từ phiếu hồ sơ lỗi).
