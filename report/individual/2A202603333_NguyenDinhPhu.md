# Báo cáo cá nhân — Ngày 2 Object Detection

## 1. Thông tin học viên

- Họ tên: Nguyễn Đình Phú
- MSSV: 2A202603333
- Email: nguyendinhphu200@gmail.com
- Lớp: K5-L2B
- Link repo bài làm: https://github.com/nguyendinhphu2004/K5-L2B-Day02-NguyenDinhPhu-2A202603333-ObjectDetection
- Link Colab đã chạy: không có (Colab không cấp được GPU; đã chạy local, xem mục 5 và 6)

## 2. Schema và mini guideline

Chi tiết trong [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md).

Các quyết định mơ hồ chính (xem bảng đầy đủ trong guideline):

| Ảnh         | Tình huống                                         | Quyết định                               |
| ----------- | -------------------------------------------------- | ---------------------------------------- |
| `drive_014` | Xe cứu hộ/kéo xe có cần cẩu phía sau               | `truck` (xe tải chuyên dụng)             |
| `drive_024` | Xe ba bánh chở người/hàng                          | Không gán (ngoài bốn lớp)                |
| `drive_030` | MPV/minivan cỡ lớn, nằm giữa `car` và `van`        | Tạm gán `van`, ghi lại để hỏi giảng viên |
| `drive_042` | Xe tải và xe buýt chồng nhau, xe buýt bị cắt khung | Hai box riêng trên phần nhìn thấy        |
| `drive_012` | Minibus, khó phân biệt `bus` và `van`              | Tạm gán `bus`, ghi lại để hỏi giảng viên |

Phần cần giảng viên làm rõ: ranh giới `van`/`bus` (minibus) và `car`/`van` (MPV cỡ lớn).

## 3. Annotation và self-QC

- Số file nhãn train: 50 (`dataset/labels/train/`, cùng basename với ảnh).
- Kết quả checker: `python3 tools/check_yolo_labels.py` báo ĐẠT cấu trúc 50/50 file nhãn hợp lệ.
- Danh sách ít nhất 10 ảnh đã rà bằng mắt: `drive_001`, `drive_002`, `drive_003`, `drive_004`, `drive_005`, `drive_006`, `drive_007`, `drive_008`, `drive_009`, `drive_010`
- Commit khóa nhãn ban đầu: `1ad8423` ("Khóa nhãn").

## 4. Gold QC và rework

Kết quả chấm lần đầu trên bộ nhãn đã khóa (commit `1ad8423`), chạy `tools/evaluate_annotations.py` với gold đặt ngoài repo:

| Chỉ số              | Trước sửa |           Sau sửa nếu có |
| ------------------- | --------: | -----------------------: |
| TP                  |       481 | Không chạy (xem ghi chú) |
| FP                  |       606 | Không chạy (xem ghi chú) |
| FN                  |       172 | Không chạy (xem ghi chú) |
| Precision           |    0.4425 | Không chạy (xem ghi chú) |
| Recall              |    0.7366 | Không chạy (xem ghi chú) |
| F1                  |    0.5529 | Không chạy (xem ghi chú) |
| mean_iou_of_matches |    0.8396 | Không chạy (xem ghi chú) |
| passed              |           | Không chạy (xem ghi chú) |

Ghi chú về lý do: theo đánh giá của tôi, bộ gold dùng để chấm (`gold_train_labels`) có vấn đề: thiếu nhiều box (xe nhìn thấy rõ trong ảnh nhưng không có nhãn) và có box bị gán sai lớp. Số liệu hỗ trợ cho phần "thiếu box": tổng số box nhãn của tôi là 1087, gold chỉ có 653; ví dụ lệch rõ ở `drive_002` (46 box so với 13) và `drive_028` (21 so với 3). Vì F1 bị kéo xuống chủ yếu bởi FP (606) trong khi mean IoU đạt 0.8396, tôi cho rằng sửa nhãn theo gold sẽ làm nhãn kém đi thay vì tốt lên.

## 5. Kết quả YOLO

Chạy local trên máy cá nhân (không dùng Colab vì Colab báo lỗi "Failed to assign a backend: No backend with GPU available", kể cả khi đổi sang tài khoản khác).

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch: GPU, NVIDIA GeForce RTX 3050 Laptop (4096 MiB), CUDA; Ultralytics 8.4.208, Torch 2.6.0+cu124, Python 3.12, Windows.

| Chỉ số trên 10 ảnh eval | Giá trị |
| ----------------------- | ------: |
| Precision               |  0.3135 |
| Recall                  |  0.3093 |
| mAP50                   |  0.2736 |
| mAP50–95                |  0.1997 |

Nguồn: [assets/model_eval_metrics.json](assets/model_eval_metrics.json). Ảnh dự đoán 10 ảnh eval: [assets/eval_predictions/visuals/](assets/eval_predictions/visuals/) (ví dụ [drive_051.jpg](assets/eval_predictions/visuals/drive_051.jpg)). Log train: [assets/yolo26_runs/vehicle_annotation/results.csv](assets/yolo26_runs/vehicle_annotation/results.csv); confusion matrix và đường PR: [assets/yolo26_runs/vehicle_eval/](assets/yolo26_runs/vehicle_eval/).

Trả lời bốn câu hỏi cuối notebook:

1. **Lớp recall thấp nhất:** `bus` (R = 0, chỉ 2 vật thể trong eval), sau đó `truck` (R ≈ 0.2, 5 vật thể) và `van` (R ≈ 0.25, 12 vật thể). Theo [confusion matrix](assets/yolo26_runs/vehicle_eval/confusion_matrix.png), 4/5 `truck` bị đoán thành `car` và 11/12 `van` bị đoán thành `bus`. Ảnh minh họa: [`drive_051`](assets/eval_predictions/visuals/drive_051.jpg), nhãn eval có một `bus` ở góc trên bên phải (xe thùng trắng) nhưng model không đoán ra `bus`. Số vật thể mỗi lớp rất ít nên kết quả dao động mạnh.
2. **mAP50 cao nhưng mAP50–95 thấp:** cần kiểm độ khít của box ở các ngưỡng IoU cao (cạnh box lệch/lỏng, quy tắc vẽ box có nhất quán với xe bị che, bị cắt khung, bóng đổ), bằng cách đặt box dự đoán cạnh box thật trên ảnh. Ở bài này mAP50 (0.2736) và mAP50–95 (0.1997) không cách nhau quá xa, nên hạn chế chính là phân loại và độ phủ hơn là độ khít box.
3. **Lỗi class liên quan guideline:** nhầm `van` ↔ `bus` và `truck` → `car`, đúng là các ranh giới mơ hồ ghi trong `GUIDELINE_MINI.md` (minibus/MPV). Dữ kiện: 11/12 `van` thật bị đoán thành `bus`; ở [`drive_053`](assets/eval_predictions/visuals/drive_053.jpg) model đoán `bus` (0.86, 0.84) cho hai xe buýt rõ ràng nhưng nhãn eval của hai xe này là `van`, nên một phần lỗi có thể do nhãn chứ không chỉ do model. Tập train của tôi cũng lệch lớp (935 `car`, 101 `bus`, 38 `truck`, chỉ 13 `van`), và ở `drive_051` model đoán một chiếc sedan là `van` với độ tin cậy 0.99, cho thấy lớp `van` chưa học ổn định.
4. **Không train bằng eval:** để đo khả năng tổng quát hóa trên ảnh chưa thấy; đưa eval vào train sẽ làm mAP bị thổi phồng do rò rỉ dữ liệu. Trong notebook, 10 ảnh eval là tập **validation**: `data.yaml` trỏ `val` tới `images/eval`, dùng để đánh giá sau mỗi epoch và chọn `best.pt`, nên không phải test set độc lập.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm       | Lỗi quan sát                                                                              | Cách đã thử                                 | Kết quả/còn thiếu               |
| -------------------- | ----------------------------------------------------------------------------------------- | ------------------------------------------- | ------------------------------- |
| Chạy YOLO trên Colab | `Failed to assign a backend: No backend with GPU available` (T4 và tài khoản mới đều lỗi) | Thử đổi runtime, đổi tài khoản              | Chạy local trên RTX 3050 Laptop |
| Chấm gold lần đầu    | F1 0.5529 (< 0.80), nhiều FP                                                              | Phân tích theo ảnh và theo nhóm tự gán/YOLO | Cần rà lại ảnh để rework        |

## 7. Tự đánh giá theo rubric

| Tiêu chí                           | Trạng thái                                                                                                         |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| Hoàn thành và định dạng YOLO       | Đã có 50 file, checker đạt                                                                                         |
| Độ bám sát và hình học box         | Mean IoU 0.84 so với gold                                                                                          |
| Mini guideline và quyết định mơ hồ | Đã điền, có 5 ca mơ hồ                                                                                             |
| Gold-set annotation QC             | Chưa đạt (F1 0.5529), cần rework                                                                                   |
| Pipeline YOLO và phân tích         | Đã chạy local (mAP50 0.2736, mAP50–95 0.1997); chưa viết phân tích 4 câu hỏi; train bằng nhãn chưa đạt ngưỡng gold |

## 8. Công cụ AI và tài liệu tham khảo

- Trợ lý AI (Claude): giải thích quy trình dùng CVAT; chạy checker và evaluator; phân tích số liệu FP/FN. Phần tôi cần tự kiểm tra: nội dung guideline, các ca mơ hồ, và việc rà nhãn bằng mắt.
- Tài liệu: README, GUIDE, RUBRIC, SUBMISSION của repo bài lab; tài liệu CVAT.
