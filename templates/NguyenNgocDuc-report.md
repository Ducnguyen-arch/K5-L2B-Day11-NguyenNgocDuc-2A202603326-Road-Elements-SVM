# Báo cáo cá nhân — Ngày 11
Họ tên: Nguyễn Ngọc Đức               Mã học viên: 2A202603326  
Phiên bản CVAT (Help → About): 2.81.0 (điền theo Help → About trên CVAT lớp)  
Guideline: Road v1.0    Lần nộp (`revision` trong manifest): 1  
Thời điểm export (ngày giờ trên CVAT): front 2026-11-21 11:11, fisheye 2026-11-21 11:25   Số lần nộp lại sau Rework: 0  

## 1. Kiểm project
| Mục | Đúng như guideline Road v1.0? | Ghi chú |
| --- | --- | --- |
| 6 label, đúng kiểu | Đúng | lane, road_curb: polyline; crosswalk, drivable: polygon; traffic_light, traffic_sign: rectangle (đọc từ `<meta>` của `annotations.xml`) |
| Thuộc tính và giá trị | Đúng | lane: position L1/R1/L2/R2/other, style solid/dashed, color white/yellow/other, kind single/double. drivable: area_type direct/alternative. traffic_light: color red/yellow/green/off/unknown, type vehicle/pedestrian/beacon, facing front/side/back, edge_zone (checkbox). traffic_sign: group 6 giá trị, facing, edge_zone |
| 4 tag ảnh | Đúng | camera (front, fisheye_front, fisheye_rear, fisheye_left, fisheye_right), timeofday (day/dusk/night), weather (clear/overcast/rain/fog/other), source (real/simulated); mỗi tag có thuộc tính select `value` |

Cách kết thúc polyline trên bản CVAT này: đặt điểm cuối rồi bấm **N** lần nữa (hoặc nút Done trên thanh công cụ) để kết thúc; sau đó mở Details sửa thuộc tính, vì CVAT tự điền `L1 / solid / white / single`.  
Cách đặt tag ảnh trên bản CVAT này: bấm nút **Setup tag** ở thanh bên trái, chọn label tag (camera trước), bấm Add; sau đó kiểm lại `value` trong Details, vì CVAT tự chọn giá trị đầu danh sách (ví dụ `weather = clear`).  

## 2. Số lượng đã gán
| Task | Ảnh | lane | road_curb | crosswalk | drivable | traffic_light | traffic_sign |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| front | 12 | 55 | 22 | 6 | 34 | 39 | 44 |
| front-extra (đã làm 0 / 4) | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| fisheye | 8 | 15 | 18 | 12 | 11 | 8 | 17 |
| fisheye-extra (đã làm 0 / 4) | 0 | 0 | 0 | 0 | 0 | 0 | 0 |

Nguồn số liệu: `shapes_by_label` trong `NguyenNgocDuc-manifest.json` (revision 1).

## 3. Bảng ca khó
| Ảnh | Nhãn | Phân vân giữa | Đã chọn | Lý do |
| --- | --- | --- | --- | --- |
| A04 | lane L2 (#16 → #12), vàng đôi | Một polyline style solid hay dashed cho vạch đôi một liền một đứt (QCVN 1.4) | Tách thành hai polyline: đoạn gần solid, đoạn xa dashed, cùng kind = double, color = yellow | Road v1.0 không có giá trị style riêng cho vạch 1.4 (guideline mục 6); giữ đúng kiểu vạch ở từng đoạn |
| A04 | lane R2 (#0, #22) | Vạch đôi là một polyline hay hai nét | Hai polyline song song | Chưa chắc; theo quy tắc kind = double thì nên gộp thành một polyline. Sẽ sửa ở lần nộp sau |
| A08 | lane #39 (gần nằm ngang, bên trái) | Vạch làn L1 hay vạch dừng / mép vạch ngựa vằn | position = other | Nằm ngang qua làn, không chạy về điểm tụ như vạch làn |
| A09 | lane L1 (#5), L2 (#6), kind double | Hai nét của một vạch đôi hay hai vạch riêng | Hai polyline, L1 và L2 | Hai nét cách nhau ~45 px ở đáy, chụm lại ở xa; cần coach xác nhận |
| A12 | lane L1, 8 polyline | Vạch đứt bị tách theo từng vệt hay vệt vạch ngựa vằn | Đang để L1 | Chưa chắc; lint báo QC1-05. Cần xem lại trên ảnh |
| A01, A03, A05, A06 | lane L1 / R1 | Có thấy vạch sát xe hay không | Không vẽ thêm vạch | Không thấy vạch rõ; cần coach xác nhận để được miễn QC1-02 |

## 4. Phiếu QC 1 nhận được

| Mã lỗi | Số lỗi | Đồng ý? | Đã sửa? |
| --- | ---: | --- | --- |
| QC1-01 tag camera (nặng) | 4 | Đồng ý | Đã sửa: cả 12 ảnh front có đủ 4 tag |
| QC1-01 tag phụ (nhẹ) | 12 | Đồng ý | Đã sửa |
| QC1-03 position (nặng) | 5 | Đồng ý | Đã sửa một phần: A10 đổi lại L1/L2; A08 vạch ngang → other, đổi lại L1/L2; A12 #8 → L1. A08 #20 vẫn là L1 nằm ngang, chưa sửa |
| QC1-04 chiều vẽ (chính) | 1 | Đồng ý | Đã sửa (A12, vẽ lại từ gần ra xa) |
| QC1-05 vạch đứt, vạch đôi (chính) | 3 | Đồng ý một phần | Chưa sửa (A04 R2 và L1, A12 L1) |

## 5. Kiểm file export
| File | Mở lại được? | Số shape ảnh 1 (job / file) | Số shape ảnh 2 (job / file) | Tag camera có trong XML? |
| --- | --- | --- | --- | --- |
| `NguyenNgocDuc-front.zip` | Có, `road_check lint` mở được; zip chỉ có `annotations.xml` | A01: 10 / 10 | A02: 9 / 9 | Có, 12/12 ảnh `camera = front` |
| `NguyenNgocDuc-fisheye.zip`| Có; zip chỉ có `annotations.xml` | B01: 6 / 6 | B02: 9 / 9 | Có ở 8/8 ảnh, nhưng B01, B02 đang là `front` (phải là `fisheye_front`) và B06 là `fisheye_right` (phải là `fisheye_left`) |


## 6. Ảnh chụp màn hình
1. Tab Labels của project. ![labels](screenshots/01_labels.png)
2. Một ảnh camera trước đã gán đủ (làn, vùng lái, đèn/biển). ![front](screenshots/02_front_A06.PNG)
3. Một ảnh fisheye đã gán, phóng to đoạn polyline cong. ![fisheye](screenshots/fisheye_B03.png)
4. Một đoạn XML có 
<box label="traffic_light" source="manual" occluded="0" xtl="515.39" ytl="146.76" xbr="576.15" ybr="176.07" z_order="0">
<attribute name="color">green</attribute>
<attribute name="type">vehicle</attribute>
<attribute name="facing">front</attribute>
<attribute name="edge_zone">false</attribute>
</box>

## 7. Tự đánh giá
Ở lần lint đầu, 16/20 ảnh không có tag nào; sau khi sửa, fisheye vẫn còn 3 ảnh sai camera (B01, B02 gán `front`, B06 gán nhầm phải/trái). Cách tránh: làm đúng thứ tự guideline, đặt 4 tag **trước tiên** ở mỗi ảnh, đọc camera từ hậu tố tên file (FV/RV/MVL/MVR), và chạy `road_check lint` sau mỗi task chứ không đợi tới lúc export.  
Nhãn của bạn đi vào model ở đâu? Chọn một lỗi đã sửa và nói nó làm model học sai thế nào: Nhãn lane đi vào model phát hiện làn (ví dụ row-anchor), đầu ra dùng cho giữ làn (LKA). Ở A10, gán ngược L1 và L2: vạch sát xe bên trái bị gọi là L2, vạch ngoài cùng là L1. Model học theo nhãn này sẽ đặt biên trái của làn đang đi ra ngoài một làn.
## 8. (Tùy chọn, buffer) Từ polyline đến đích row-anchor
Vạch dùng: A02, lane R2 (vạch duy nhất của A02 chạm đáy ảnh y = 720), nội suy tuyến tính giữa các điểm.

| y | 720 | 680 | 640 | 600 | 560 |
| --- | ---: | ---: | ---: | ---: | ---: |
| x của chúng tôi | 1153 | 1094 | 1035 | 976 | 912 |
| x của bạn kia | — | — | — | — | — |

Trả lời 3 câu:

1. Polyline R2 bắt đầu ở y = 720, nên hàng bị mất khi dừng sớm 40 px nằm ở **đầu xa**: điểm xa nhất hiện ở y ≈ 404, dừng sớm 40 px thì các hàng từ ~404 tới ~444 thành "không có làn". Năm hàng 720–560 không đổi. Nếu bỏ sót 40 px ở **đầu gần** (bắt đầu từ y = 680), hàng 720 sẽ thành "không có làn", đúng hàng quan trọng nhất.
2. Không có bạn được kiểm nên không so được. Theo slide 25, lệch ở hàng 720 (gần xe) ảnh hưởng LKA nhiều hơn hàng 560, vì vị trí ngang của xe trong làn được tính từ phần gần nhất.
3. Giá trị x tại mỗi hàng không đổi nếu script nội suy theo y không quan tâm thứ tự điểm. Script giả định điểm đầu là điểm gần xe (lấy điểm đầu làm gốc vạch, hoặc nội suy theo thứ tự điểm tăng dần của y) sẽ gặp khó: `road_check lint` báo QC1-04, và bước chuyển đổi sang đích row-anchor phải đảo lại điểm trước khi tính.
