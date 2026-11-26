# Báo cáo Day 13 — Đào Quang Tuấn, 2A202603194

## 1. Kết luận trước
Mã số 2A202603194, task `D13-2A202603194`, 10 frame: s01f03, s01f24, s02f23, s02f52, s03f12, s03f44, s04f08, s04f52, s05f07, s05f42. Máy Windows 11, trình duyệt Chrome, CVAT v2.82.1 (chạy trên máy cá nhân). Ngồi cạnh: 2A202603202.

Tổng **141 cuboid** (101 Car, 23 Pedestrian, 12 Truck, 4 Bus, 1 Cyclist); frame ít nhất là s03f44 (6 cuboid): đường thẳng có dải phân cách cây xanh và hàng rào hai bên, trong 50 m chỉ có vài xe cùng chiều phía trước và một xe phía sau. Lần `sync` cuối `selfcheck` cờ **9/141** cuboid. Tôi tin nhất phần **hướng** (xe dọc đường đều có yaw lệch 90°/270° dưới khoảng 6°) và **đáy** (đa số khoảng hở với mặt đường dưới 0,1 m); yếu nhất là **class** (người ngồi xe máy gán nhầm Pedestrian, van chở hàng gán Car), **occluded** (chưa bật cho hộp nào) và **vật xa thưa điểm**.

Cách dùng CVAT:
- Đổi góc nhìn: kéo chuột trái trong khung 3D để xoay tới khi nhìn thẳng xuống, cuộn để thu phóng, kéo chuột phải hoặc phím mũi tên để dời; U/I/O, J/K/L để xoay/nâng hạ camera.
- Vẽ và chỉnh cuboid: chọn công cụ cuboid → **Shape**, chọn class, bấm vào cụm điểm; nháy đúp vào hộp để mở ba khung **Top/Side/Front**, kéo các điểm đỏ cho sát điểm LiDAR, dùng nút xoay để chỉnh hướng.
- Bật **Cuboid orientation** trong Settings để thấy vạch đỏ hướng (+X của hộp); xem cuboid trên ảnh camera bằng `python scripts/lab.py preview` (CVAT của lớp không có nút chiếu box lên ảnh).

## 2. Đọc frame (Bước 2)
Hai frame đọc kỹ. Ảnh: `t1-s01f03-3d`, `t1-s01f03-top`, `t1-s01f24-3d`, `t1-s01f24-top`.

| Frame | Phía trước xe ở cạnh nào của ảnh nhìn từ trên xuống; +x, +y chỉ về đâu | Mặt đường (phẳng/dốc; thấp hơn LiDAR ~ m) | Vật xa nhất đọc được (~ m) | Loại vật thấy | Vật phía sau / hai bên | Cụm chưa đọc được |
|---|---|---|---|---|---|---|
| s01f03 | Phía trước (−y) ở cạnh trên ảnh top toàn cảnh; +x chỉ sang trái ảnh (bên trái xe); +y chỉ xuống cạnh dưới (sau xe) | Phẳng, thấp hơn LiDAR khoảng 1,7 m | Khoảng 45–50 m (xe con rìa trên-trái, 2 xe con rìa dưới-trái và xe lớn ở góc dưới-phải) | 16 xe con, 1 xe tải vừa, 1 xe lớn (xe buýt hoặc xe tải thùng dài), 1 người hoặc xe hai bánh; xung quanh có cây cối và tường/công trình hai bên | Sát phải 1 xe con (~3–4 m); trước-trái 1 xe con (~10 m), sau-trái 1 xe con (~8 m); bên trái 6 xe con và 1 xe tải (~12–20 m); phía sau 2 xe con (~12–18 m), 1 cụm nhỏ sau-phải (~20 m), 1 xe con sau-phải (~28 m), 1 xe lớn góc dưới-phải (~45 m) | Cụm nhỏ sau-phải ~20 m: thấp, ít điểm, không rõ là người, xe hai bánh dựng hay cột/bồn cây |
| s01f24 | Như s01f03: phía trước (−y) ở cạnh trên; +x sang trái ảnh; +y xuống cạnh dưới. Xe đang ở ngã tư (vạch qua đường trước và bên phải) | Gần xe phẳng, thấp hơn LiDAR ~1,7 m; dốc nhẹ: phía trước-trái xa thấp hơn ~0,6 m (đáy xe ~−2,3 m), phía sau-trái xa cao hơn ~0,5 m (đáy xe ~−1,2 m) | Khoảng 39 m (xe con sau-trái) | 12 xe con, 1 xe buýt, 4 người (2 trong số đó có vẻ ngồi trên xe máy, xem §6c); cây, nhà cao tầng, cột đèn | Sau-trái xe buýt (~24 m) và 3 xe con (~6, 12, 19 m); trước-trái 1 xe con (~6 m); bên phải 1 người/xe máy (~7 m), trước-phải 1 người/xe máy (~15 m) và 1 xe con (~11 m) | Cụm 3 điểm sau-trái ~39 m (đã vẽ rồi xoá, xem §3) |

Chỉ **camera** cho biết: (1) loại xe cụ thể (sedan hay SUV, xe buýt có biển tuyến) và màu xe; (2) đầu/đuôi xe đang đứng yên ở ngã tư s01f24 (đèn hậu, kính lái); (3) người ở bên phải s01f24 đang ngồi trên xe máy chứ không đi bộ, và đèn giao thông đang xanh. Chỉ **LiDAR** cho biết: (1) khoảng cách thật tới từng vật bằng mét (ví dụ xe con trước-phải s01f03 cách 8,9 m); (2) mặt đường ở s01f24 dốc lên/xuống bao nhiêu mét; (3) vật phía sau và ở xa 40–50 m mà ảnh chỉ thấy vài pixel hoặc bị che.

Tám frame lướt:

| Frame | Phía trước ở cạnh nào của ảnh nhìn từ trên xuống | Điều lạ (nếu có) |
|---|---|---|
| s02f23 | Cạnh trên (−y) | Hai xe con bên phải ~32–33 m và xe tải ~44 m có đáy cao hơn mặt đường quanh xe thu khoảng 2 m (giả thuyết: đường bên phải nằm trên cao/dốc lên); một xe con chỉ 5 điểm |
| s02f52 | Cạnh trên (−y) | Chỉ 7 xe con, hai chiều xe chạy (yaw ~90° và ~270°), không có người |
| s03f12 | Cạnh trên (−y) | Xe con phía sau ~17,7 m chỉ 57 điểm vì bị xe sát sau che; người phía trước ~24 m chỉ 8 điểm |
| s03f44 | Cạnh trên (−y) | Ít vật nhất (6); xe con phía trước ~18,6 m chỉ 40 điểm vì bị xe đen ~9 m che |
| s04f08 | Cạnh trên (−y) | Xe tải phía trước ~47,8 m sát ranh 50 m, 45 điểm, `selfcheck` không ước được mặt đường quanh nó |
| s04f52 | Cạnh trên (−y) | Hai người ~28 m và ~36 m chỉ 14–17 điểm |
| s05f07 | Cạnh trên (−y) | Nhiều vật nhất (20); một xe con ~37 m bị `selfcheck` báo chìm 1,02 m (§5) |
| s05f42 | Cạnh trên (−y) | Các xe bên trái ~41–44 m có đáy ~−3,2 m, thấp hơn ~1,5 m (giả thuyết: đường dốc xuống); van trắng sát bên trái rất to so với xe con |

## 3. Ba cuboid có hướng dẫn (Bước 3)
Ảnh: `t2-a-car`, `t2-b-bus-truck`, `t2-c-ped-cyclist`.

| Vật | Frame, idx | Class | Mũi tên yaw đúng hướng? | Đáy chạm đường? | Đã kiểm Top/Side/Front + ảnh? | Kích thước trong bảng 4.3? |
|---|---|---|---|---|---|---|
| (a) xe con gần | s01f03, 16 | Car | Có: yaw 266,4°, gần 270°, xe cùng chiều phía trước, vạch đỏ chỉ về đầu xe (−y) | Có: z đáy = −1,050 − 1,570/2 = −1,835 m, lệch mặt đường ~−1,7 m khoảng 0,13 m | Có, đã kiểm Top/Side/Front; đối chiếu cam09 | Có: 4,42 × 1,86 × 1,57 m, trong khoảng sedan 3,8–5,0 × 1,70–1,90 × 1,40–1,60 m |
| (b) xe buýt hoặc xe tải | s01f03, 10 | Truck | Có: yaw 88,2°, gần 90°, xe đi ngược chiều, vạch đỏ chỉ về đầu xe (+y) | Có: z đáy = −0,310 − 3,310/2 = −1,965 m, lệch khoảng 0,27 m (trong ngưỡng 0,3 m) | Có, đã kiểm Top/Side/Front | Dài 8,75 m và cao 3,31 m trong khoảng xe tải hộp (5,5–9,0 m; 2,40–3,50 m); **rộng 2,81 m vượt khoảng 2,00–2,55 m**. Giữ nguyên vì hộp khớp cụm điểm trên Top; giả thuyết: hộp bao cả gương chiếu hậu hoặc phần thùng chìa ra |
| (c) người đi bộ hoặc xe hai bánh | s01f03, 9 | Pedestrian | Người đứng yên: căn theo hướng mặt, yaw 321,4° | Có: z đáy = −0,660 − 1,800/2 = −1,560 m, lệch khoảng 0,14 m | Có, đã kiểm Top/Side/Front | Có: 0,85 × 0,75 × 1,80 m, trong khoảng người lớn 0,4–0,9 × 0,5–0,9 × 1,50–1,95 m |

Vòng hiệu chuẩn: không tham gia.

Vật bỏ qua vì không đủ cơ sở (đã vẽ rồi xoá sau `selfcheck`):
- s01f24, người đi bộ phía sau bên trái, cách khoảng 38,9 m: chỉ 3 điểm LiDAR, không đủ để xác định là người: **LOW_VISIBILITY**.
- s05f07, người đi bộ phía trước hơi lệch phải, cách khoảng 46,8 m (sát ranh 50 m): chỉ 4 điểm: **LOW_VISIBILITY** (và UNCERTAIN_SCOPE vì gần ngưỡng 50 m).

## 4. Bốn hiện tượng chất lượng (Bước 4)
Nguyên nhân ghi là **giả thuyết**.

| Hiện tượng | Frame / tên ảnh / nguồn | Nguyên nhân (giả thuyết) | Thấy gì trên point cloud | Hệ quả khi gán (theo guideline) |
|---|---|---|---|---|
| 1. Bóng che | s03f44, `t4-1-bong-che`, frame của tôi | Xe đen ~9 m ngay phía trước chắn tia LiDAR tới xe phía sau nó | Xe con ngay sau đó (~18,6 m) chỉ còn 40 điểm, chủ yếu phần nóc; trong khi xe cùng làn ~22 m lệch sang bên có 296 điểm. Vùng ngay sau xe đen gần như trống: đó là vùng **chưa quan sát được**, không phải đường trống | Bật **occluded** cho xe bị che, cuboid vẫn bao trọn phần suy luận được (dài theo kích thước xe con, đáy chạm đường). Tôi chưa bật occluded cho hộp nào: cần sửa |
| 2. Thưa ở xa | s01f24, `t4-2-thua-o-xa`, frame của tôi | Tia LiDAR toả ra, càng xa khoảng cách giữa các tia càng lớn | Xe con sau-trái ~39,4 m chỉ 7 điểm, rải trên vài đường quét | Vẽ nếu thấy khoảng 15–20% thể tích hoặc suy luận được (ở đây có ảnh camera rõ). Không đủ thì bỏ qua, ghi LOW_VISIBILITY (như hai người 3–4 điểm ở §3) |
| 3. Phản xạ thấp / ít điểm bất thường | s02f23, `t4-3-phan-xa-thap`, frame của tôi | Thùng xe tối màu hoặc bề mặt phản xạ yếu nên ít tia quay về (cũng có thể bị xe phía trước che một phần) | Xe tải phía trước ~41,2 m chỉ 30 điểm, trong khi xe tải ~43,7 m cùng frame (xa hơn) có 69 điểm và xe con ~32,4 m có 134 điểm | Không nới hay thu hộp theo số điểm; dùng ảnh camera và bảng 4.3 để chốt kích thước; ghi là giả thuyết, không khẳng định |
| 4. Điểm lạ | s02f35, `t4-4-diem-la`, task chung `D13-HC485087` | Phản xạ nhiều lần (tia dội từ bề mặt bóng như kính, nước) tạo điểm ảo | Cụm khoảng 46 điểm sau-trái ~29 m nằm ở z ≈ −13,5 m, thấp hơn mặt đường quanh đó ~11 m; chỉ thấy rõ khi xoay nghiêng khung 3D | Không gán thành vật nào |

## 5. Số từ cuboid của tôi (Bước 5)
Lấy từ `submission/D13-2A202603194-bangA.csv`; số điểm lấy từ cột `points_in` của `-tukiem.csv`. Mặt đường khoảng −1,7 m: z đáy lệch hơn 0,3 m là lơ lửng hoặc chìm.

| Cuboid (frame, idx, class) | z | h | z đáy = z − h/2 | dist_m | r = √(x²+y²+z²) | yaw_deg | Nhận xét (lơ lửng / chìm / chạm đường) |
|---|---|---|---|---|---|---|---|
| s01f03, 16, Car | −1,050 m | 1,570 m | −1,835 m | 8,89 m | 8,95 m | 266,4° | Chạm đường (lệch −1,7 m khoảng 0,14 m; `selfcheck` đo khoảng hở 0,02 m) |
| s05f07, 3, Car | −0,545 m | 1,970 m | −1,530 m | 36,59 m | 36,60 m | 88,2° | So với −1,7 m thì chỉ cao 0,17 m (chạm), nhưng `selfcheck` báo chìm 1,02 m so với mặt đất nó ước quanh hộp; đã giữ nguyên (xem bảng tự kiểm) |
| s03f44, 2, Pedestrian | 0,830 m | 1,780 m | −0,060 m | 22,69 m | 22,71 m | 285,3° | So với −1,7 m thì lơ lửng 1,64 m, nhưng khoảng hở với điểm đất ngay quanh hộp chỉ 0,08 m: giả thuyết người đứng trên nền cao hơn mặt đường (bậc, dốc lên) |

(a) Cuboid **gần nhất** trong bảng A là s03f12 #17 Car: `dist_m` 0,26 m, r 0,89 m. Hộp này (cùng s01f03 #18 và s01f24 #18) thực chất bao **xe thu dữ liệu**, là lỗi (xem §6c). Vật thật gần nhất là s05f42 #13 Car: `dist_m` 3,85 m, r 3,97 m. **Xa nhất**: s01f03 #1 Car, `dist_m` 48,95 m, r 48,96 m. r chỉ lớn hơn `dist_m` chút ít vì z nhỏ so với khoảng cách ngang.

(b) Người đi bộ s01f03 #9: 0,85 × 0,75 × 1,80 m, cả ba chiều trong khoảng người lớn. Xe con s05f42 #12: 5,43 × 2,12 × 2,15 m, **cả ba chiều vượt** khoảng SUV/van chở người (≤ 5,3 × 2,10 × 2,10 m): ảnh cam01/cam06 cho thấy đây là van trắng thân kín, có vẻ là van chở hàng nên đúng ra là **Truck** (UNCERTAIN_CLASS). Ngược lại, xe con s01f03 #16 (4,42 × 1,86 × 1,57 m) nằm gọn trong khoảng sedan.

(c) Xe cùng chiều phía trước s01f03 #16: yaw 266,4°, gần 270° (lệch 3,6°). Xe ngược chiều s01f03 #13: yaw 92,2°, gần 90° (lệch 2,2°).

Mật độ theo khoảng cách (4 xe con cùng frame s01f03, không bị che):

| Cuboid (frame, idx, class) | dist_m | Số điểm (`points_in`) |
|---|---|---|
| s01f03, 16, Car | 8,89 m | 1433 |
| s01f03, 11, Car | 19,19 m | 683 |
| s01f03, 5, Car | 27,71 m | 307 |
| s01f03, 1, Car | 48,95 m | 62 |

Xa gấp khoảng 2 lần thì số điểm còn khoảng một nửa (8,9 → 19,2 m: 1433 → 683), xa gấp 5,5 lần thì còn khoảng 1/23 (8,9 → 49 m: 1433 → 62), tức giảm nhanh hơn tỉ lệ nghịch với khoảng cách, gần 1/d². Khớp đường cong ở slide: 683 điểm ở 19 m sát mức 718 của 10–20 m; 62 điểm ở 49 m nằm giữa 72 (30–40 m) và 19 (60–80 m); xe 27,7 m có 307 điểm, cao hơn mức 214 của 20–30 m, giả thuyết vì đó là SUV cao 1,96 m và quay ngang (yaw 5,7°) nên lộ cả cạnh dài về phía LiDAR.

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
| s01f24 #16 Pedestrian (lần 2: #15) | 157 điểm sát ngoài mép | **Giữ nguyên:** cờ "điểm sát ngoài mép" gặp cả ở nhãn chuẩn với người, xe hai bánh (22–28%, theo `docs/SELF_CHECK.md`). Xem lại sau: có vẻ là người ngồi xe máy (§6c) | Còn cờ |
| s02f23 #6 Pedestrian | 11 điểm sát ngoài mép | **Giữ nguyên:** như trên | Còn cờ |
| s02f23 #8 Pedestrian | 30 điểm sát ngoài mép | **Giữ nguyên:** như trên | Còn cờ |
| s02f23 #11 Cyclist | 50 điểm sát ngoài mép | **Giữ nguyên:** như trên | Còn cờ |
| s03f12 #10 Pedestrian | 25 điểm sát ngoài mép | **Giữ nguyên:** như trên | Còn cờ |
| s03f44 #2 Pedestrian | 20 điểm sát ngoài mép | **Giữ nguyên:** như trên | Còn cờ |

Sau khi sửa và chạy lại `sync` (lần cuối): bảng A còn **141 cuboid** (xoá 3 hộp: 1 hộp trùng, 2 hộp quá ít điểm), `selfcheck` còn cờ **9/141** cuboid. Hai cuboid sửa rõ nhất: s01f03 #2 Bus (width 3,11 m → 2,50 m) và s01f03 #14 Car (z_bottom −1,405 m → −1,883 m, hết lơ lửng).

Đổi kiểm với 2A202603202: bạn ấy đối chiếu `dist_m` và `yaw_deg` của s01f03 #16 Car với bảng A. `yaw_deg` 266,4° khớp; `dist_m` **lệch 0,4 m**. Tôi đã đối chiếu lại và sửa số trong báo cáo cho đúng bảng A lần `sync` cuối: `dist_m` 8,89 m (bảng ở §5).

## 6. Ý nghĩa của nhãn, cách dùng AI và điều còn chưa chắc
(a) Model 3D phải dự đoán vật ở đâu và to bao nhiêu **trong thế giới thật** để xe phanh, tránh đúng; cùng một xe con có kích thước pixel thay đổi theo khoảng cách và camera, còn kích thước mét thì không đổi, khớp với LiDAR và kiểm được bằng bảng 4.3, nên nhãn đo bằng mét mới dạy model đúng hình học. Vật thưa điểm (vài điểm ở 40 m) nếu mỗi người gán một kiểu thì model học phải nhãn mâu thuẫn, hoặc học vẽ hộp cho cả cụm nhiễu; quy tắc 15–20% thể tích cộng LOW_VISIBILITY giữ cho nhãn nhất quán giữa các người gán.

(b) Dùng AI:
- Công cụ: Claude Code (Claude) trong VS Code.
- Câu hỏi chính: viết script chỉ đọc để tìm ID trong CVAT của các hộp bị `selfcheck` cờ (`work/tim_id.py`), tính r và lọc số từ bảng A, phiếu tự kiểm; gợi ý cấu trúc câu trả lời cho báo cáo.
- Cách dùng:  mỗi hộp bị cờ tôi tự mở lại trên CVAT và ảnh `preview` trước khi sửa hoặc giữ.

(c) Điều còn chưa chắc, lỗi phát hiện sau:
- Có ba hộp Car đặt ngay tại gốc tọa độ, bao chính xe thu dữ liệu, trong khi hướng dẫn yêu cầu bỏ qua xe này; đây là lỗi, cần xoá.
- Hai hộp Pedestrian bên phải ở s01f24 có vẻ là người đang ngồi trên xe máy (ảnh camera trước-phải, sau-phải), nên đúng ra là Cyclist với hộp dài hơn để bao cả xe; điều này cũng giải thích cờ "nhiều điểm sát ngoài mép".
- Van trắng sát bên trái và một xe to bên trái xa ở s05f42 có thể là van chở hàng (Truck) chứ không phải Car.
- Chưa bật occluded cho xe nào, kể cả các xe rõ ràng bị xe phía trước che.
- Hộp xe con ở s05f07 bị báo chìm: chưa chắc lỗi nằm ở hộp hay ở cách `selfcheck` ước mặt đường; cần hỏi coach.
