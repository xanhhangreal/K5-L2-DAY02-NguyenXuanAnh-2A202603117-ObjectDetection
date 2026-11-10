# Báo cáo cá nhân — Ngày 2 Object Detection

## 1. Thông tin học viên

- Họ tên: Nguyễn Xuân Anh.
- MSSV: 2A202603117.
- Email: anhnguyenkaz@gmail.com.
- Lớp: E402
- Link repo bài làm: [GitHub](https://github.com/xanhhangreal/K5-L2-DAY02-NguyenXuanAnh-2A202603117-ObjectDetection).
- Link Colab đã chạy: [day2_annotation_to_yolo.ipynb](https://colab.research.google.com/drive/1lcm6pbzKV-bW4XsKumuJBxfG6KMq80j6?usp=sharing).

## 2. Schema và mini guideline

Schema cố định: `car=0`, `truck=1`, `bus=2`, `van=3`. Xe bán tải thuộc `car`; xe tải chở hàng/đầu kéo thuộc `truck`; xe buýt/minibus thuộc `bus`; van/minivan thuộc `van`. Mỗi xe có một box sát phần nhìn thấy; không suy đoán phần bị che hoặc nằm ngoài ảnh.

Định nghĩa và quy tắc chi tiết nằm trong [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Ba tình huống đã ghi nhận:

| Ảnh | Điểm mơ hồ | Trạng thái |
|---|---|---|
| `drive_002.jpg` | Xe trắng bên trái xe buýt lớn ở giữa ảnh; chưa phân biệt chắc loại xe. | Chưa chốt lớp, chưa có giải đáp từ giảng viên. Bản nhãn hiện tại chưa có box riêng cho xe này. |
| `drive_007.jpg` | Xe ở nửa trên ảnh, trong làn sát bên phải dải phân cách; hình xe nhỏ, mờ. | Chưa chốt lớp, chưa có giải đáp từ giảng viên. Bản nhãn hiện tại chưa có box riêng cho xe này. |
| `drive_009.jpg` | Xe màu tối ngay dưới xe khách trắng, phía sau xe buýt lớn ở tiền cảnh; khó xác định phần nhìn thấy. | Bản nhãn hiện tại có box `car`; cần rà lại phạm vi phần nhìn thấy. Chưa thực hiện rework. |

Các ca trên là ghi chép vấn đề cần xử lý, chưa được coi là ba quyết định annotation cuối cùng đã được xác nhận.

## 3. Annotation và self-QC

- Số file nhãn train: **50/50**, tổng cộng **574 box**.
- Kết quả checker: chạy `python tools/check_yolo_labels.py` từ thư mục gốc repo ngày 10/11/2026, kết quả: **ĐẠT cấu trúc: 50/50 file nhãn hợp lệ**. Lần kiểm tra này không báo lỗi định dạng. Checker không chứng minh nhãn đúng lớp, đủ vật thể hoặc box sát.
- Kiểm tra trực quan bổ sung: đã dựng ảnh chồng nhãn và đối chiếu 10 ảnh `drive_001.jpg` đến `drive_010.jpg`; ghi chú bên dưới. Học viên chưa xác nhận đã tự rà đủ 10 ảnh, nên phần kiểm tra bổ sung không được coi là xác nhận hoàn thành self-QC cá nhân.
- Cách xử lý sau kiểm tra: ghi nhận các vùng cần xem lại; chưa thay đổi file nhãn hoặc chạy rework.
- Commit lưu bộ nhãn dùng trong lần chạy Colab: [70c032e](https://github.com/xanhhangreal/K5-L2-DAY02-NguyenXuanAnh-2A202603117-ObjectDetection/commit/70c032e0f5306941d7809f488a95c670ed832861), thông điệp `lab02 day2`. Chưa có thông tin xác định thời điểm khóa nhãn theo mốc lớp so với thời điểm nhận gold.

### Ghi chú kiểm tra trực quan 10 ảnh

Các nhận xét dưới đây dựa trên ảnh chồng nhãn hiện có, là danh sách cần rà lại chứ chưa phải các lỗi đã được sửa. Không dùng số FP/FN để tự kết luận nguyên nhân cụ thể.

| Ảnh | Quan sát và việc cần kiểm tra | Ảnh chồng nhãn |
|---|---|---|
| `drive_001.jpg` | Box xe bạc gần tiền cảnh có khoảng nền ở góc trên trái do hướng xe chéo; cần đối chiếu bốn cạnh với phần xe nhìn thấy, tránh thu box chỉ để loại nền mà cắt mất xe. | [001–002](assets/annotation_qc/qc_01.png) |
| `drive_002.jpg` | Xe trắng bên trái xe khách lớn chưa có box riêng; nhiều xe xa cũng cần kiểm tra khả năng nhận diện. Giữ ca phân lớp chưa chốt. | [001–002](assets/annotation_qc/qc_01.png) |
| `drive_003.jpg` | Xe khách xanh sát mép phải và xe trắng bị cắt ở cùng mép chưa có box riêng; cần kiểm tra lớp và phần nhìn thấy trước khi bổ sung. | [003–004](assets/annotation_qc/qc_02.png) |
| `drive_004.jpg` | Cảnh đêm có xe chạm mép dưới/phải và nhiều xe máy. Cần giữ box trong biên, tránh nhầm xe máy ngoài schema thành xe bốn bánh. | [003–004](assets/annotation_qc/qc_02.png) |
| `drive_005.jpg` | Vùng xe phía xa và xe tối màu giữa ảnh mờ, có xe chồng nhau; cần kiểm tra đủ vật thể nhận diện được và box riêng cho từng xe. | [005–006](assets/annotation_qc/qc_03.png) |
| `drive_006.jpg` | Xe màu bạc gắn giá nóc sát mép phải đang mang lớp `car`; cần đối chiếu đặc điểm thân xe để phân biệt với `van`, không chốt chỉ từ giá nóc. | [005–006](assets/annotation_qc/qc_03.png) |
| `drive_007.jpg` | Xe mờ ở phía phải dải phân cách chưa có box riêng; xe xanh bên trái dải phân cách đang mang lớp `van`. Đây là hai xe khác nhau, cần tránh nhầm vị trí ca mơ hồ. | [007–008](assets/annotation_qc/qc_04.png) |
| `drive_008.jpg` | Box xe buýt lớn bao quanh cả vùng có xe khác che một phần. Cần đối chiếu từng cạnh với phần xe buýt nhìn thấy và giữ box riêng cho các xe xung quanh. | [007–008](assets/annotation_qc/qc_04.png) |
| `drive_009.jpg` | Xe tối màu giữa hai xe buýt có box `car`; cần kiểm tra cạnh dưới ở vùng bị xe buýt tiền cảnh che và độ sát box của xe buýt lớn. | [009–010](assets/annotation_qc/qc_05.png) |
| `drive_010.jpg` | Các xe phía bên kia đường có box riêng; xe ở xa bên phải khá nhỏ và một số xe bị hàng rào che. Cần kiểm tra box theo phần nhìn thấy, không đưa người đi bộ vào nhãn. | [009–010](assets/annotation_qc/qc_05.png) |

Thống kê nhãn hiện có:

| Lớp | Số box train |
|---|---:|
| car | 453 |
| truck | 20 |
| bus | 48 |
| van | 53 |
| Tổng | 574 |

## 4. Gold QC và rework

| Chỉ số | Lần đánh giá hiện có | Sau sửa nếu có |
|---|---:|---:|
| TP | 351 | Chưa có dữ liệu |
| FP | 223 | Chưa có dữ liệu |
| FN | 302 | Chưa có dữ liệu |
| Precision | 0.6115 | Chưa có dữ liệu |
| Recall | 0.5375 | Chưa có dữ liệu |
| F1 | 0.5721 | Chưa có dữ liệu |
| mean_iou_of_matches | 0.8377 | Chưa có dữ liệu |
| passed | false | Chưa có dữ liệu |

Nguồn: [annotation_evaluation.json](../../outputs/annotation_evaluation.json), matching cùng lớp tại IoU ≥ 0.50. Tổng trong JSON: 653 box gold, 574 box học viên và 351 matches; số box học viên khớp với bộ nhãn hiện có.

**Chưa đạt điều kiện annotation:** mean IoU 0.8377 đạt ngưỡng 0.70, nhưng F1 0.5721 chưa đạt ngưỡng 0.80. Đã có kết quả train model nhưng chưa thể xác nhận pipeline tuân thủ điều kiện QC trước train. Không dùng mAP model để thay thế F1 annotation.

Theo xác nhận của học viên, chưa thực hiện rework sau gold QC. Bảng trên là kết quả hiện có, chưa có cột số liệu sau sửa. Không thể kết luận toàn bộ FN là bỏ sót hoặc toàn bộ FP là box thừa; sai lớp hay box không đủ IoU cũng có thể tạo FP/FN. Các vùng cần rà đã được ghi ở mục 3; chưa sửa nhãn, chưa train lại và không thay đổi gold.

## 5. Kết quả YOLO

- Runtime: Google Colab, GPU NVIDIA Tesla T4; Python 3.13.16, Ultralytics 8.4.208, Torch 2.14.1+cu130 (theo output notebook).
- Cấu hình ghi trong lần chạy: `yolo26n.pt`, `epochs=50`, `imgsz=640`, `batch=-1`, `device=0`, `seed=20260909`. Xem [args.yaml](assets/yolo26_runs/vehicle_annotation/args.yaml). Chưa có ghi chép về lý do thay đổi tham số so với notebook lớp; báo cáo chỉ ghi cấu hình thực tế được lưu.
- Split được notebook xác nhận: 50 ảnh train và 10 ảnh eval; `eval` dùng làm validation, không đưa vào train.
- Best checkpoint trong phiên chạy: `/content/Day2-Lab/outputs/yolo26_runs/vehicle_annotation/weights/best.pt`. Checkpoint `.pt` không thuộc bài nộp GitHub.

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | 0.8882745487 |
| Recall | 0.2418013793 |
| mAP50 | 0.3772272092 |
| mAP50–95 | 0.2569528125 |

Nguồn số liệu: [model_eval_metrics.json](assets/model_eval_metrics.json). Hai thư mục kết quả đã được đặt cạnh báo cáo:

- [10 ảnh prediction](assets/eval_predictions/visuals/), từ `drive_051.jpg` đến `drive_060.jpg`.
- [Log train results.csv](assets/yolo26_runs/vehicle_annotation/results.csv).
- [Confusion matrix](assets/yolo26_runs/vehicle_eval/confusion_matrix.png).
- [PR curve](assets/yolo26_runs/vehicle_eval/BoxPR_curve.png).
- [Ảnh nhãn eval](assets/yolo26_runs/vehicle_eval/val_batch0_labels.jpg) và [ảnh dự đoán tương ứng](assets/yolo26_runs/vehicle_eval/val_batch0_pred.jpg).

### Trả lời bốn câu hỏi cuối notebook

1. **Recall thấp nhất:** output đánh giá trong notebook cho thấy `truck` và `van` cùng có recall bằng 0 tại điểm đo được báo cáo; `car` khoảng 0.467 và `bus` bằng 0.5. Ví dụ ở [drive_052.jpg](assets/eval_predictions/visuals/drive_052.jpg), chiếc xe thân dài sát lề trái có nhãn eval là `van` trong ảnh nhãn nhưng không có box tương ứng trong prediction hiển thị. Đây là ví dụ cần học viên đối chiếu lại, không phải kết luận mọi ảnh đều bỏ sót van. Recall theo lớp trong bảng metrics và hình prediction tại `conf=0.25` có thể dùng ngưỡng khác nhau, nên không suy số recall trực tiếp từ một ảnh.
2. **mAP50 cao hơn mAP50–95:** khoảng cách giữa hai chỉ số là dấu hiệu cần kiểm tra độ chính xác định vị khi tăng yêu cầu IoU: cạnh box có lỏng, lệch tâm, thiếu phần nhìn thấy hoặc lấn nền/xe bên cạnh không. Trong lần chạy này mAP50 là 0.3772 và mAP50–95 là 0.2570; không gọi mAP50 là cao tuyệt đối. Các chỉ số model không tự chứng minh nhãn train sai.
3. **Liên hệ guideline:** confusion matrix có nhầm lớp thật `truck` sang dự đoán `car`/`van`, và lớp thật `van` sang `car`/`bus`. Đây là tín hiệu để rà cách phân biệt các lớp trong guideline và nhãn train, chưa đủ để quy nguyên nhân cho guideline. Bộ train cũng lệch lớp rõ: 453 box `car` so với 20 `truck`, 48 `bus`, 53 `van`; dữ liệu ít, mất cân bằng và khác biệt cảnh có thể cùng ảnh hưởng. Không tự sửa gold eval để làm đẹp kết quả.
4. **Giữ eval ngoài train:** nếu dùng các ảnh này cập nhật trọng số, đánh giá trên chính chúng sẽ bị rò rỉ dữ liệu và không còn phản ánh khả năng dự đoán trên ảnh ngoài train. Trong notebook, 10 ảnh eval còn là validation để theo dõi quá trình học và chọn `best.pt`, nên chúng cũng không phải test set độc lập. Giữ nguyên split của bài.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| Annotation trên CVAT | Khó phân lớp hoặc xác định box khi xe mờ/bị che trong `drive_002`, `drive_007`, `drive_009`. | Ghi ca vào mini guideline để đối chiếu ảnh gốc và hỏi giảng viên. | Chưa có xác nhận quyết định cuối. |
| Kiểm tra ngày 10/11/2026 | Cần kiểm tra cấu trúc và nội dung bộ nhãn đã xuất. | Chạy checker, dựng và xem ảnh chồng nhãn cho `drive_001`–`drive_010`. | 50/50 file hợp lệ; có ghi chú 10 ảnh. Chưa xác nhận hoàn thành self-QC cá nhân, chưa rework. |
| Gold QC | F1 0.5721 thấp hơn 0.80 dù mean IoU 0.8377 đạt. | Đọc JSON và đối chiếu số box hiện có. | Chưa có kết quả chạy lại đạt; cần học viên rework theo quy định lớp. |
| Tổng hợp bài nộp | Guideline đã soạn còn ở thư mục làm việc khác, chưa nằm trong repo Desktop; assets chưa được đưa vào repo. | Chép guideline và hai thư mục kết quả vào đúng vị trí, bổ sung JSON metrics model. | Đã cập nhật file local; chưa commit/push trong bước lập báo cáo. |

## 7. Tự đánh giá theo rubric

Đối chiếu [RUBRIC.md](../../RUBRIC.md), chưa tự chấm tổng điểm:

| Hạng mục | Bằng chứng/trạng thái |
|---|---|
| Hoàn thành và định dạng YOLO | Đủ 50 nhãn, checker đạt. |
| Đầy đủ vật thể, độ sát box, phân lớp | Chưa xác nhận đạt; F1 annotation còn thấp, cần xem lại nội dung nhãn. |
| Mini guideline | Có định nghĩa, quy tắc và ba ca ghi nhận; quyết định cuối của các ca chưa xác nhận. |
| Gold-set QC | Có JSON thực tế, `passed=false`; chưa có kết quả rework đạt. |
| YOLO và phân tích | Có Colab chạy xong, metrics, log, 10 ảnh prediction và giải thích dựa trên kết quả lưu. |
| Hồ sơ kiểm tra trực quan | Có ảnh chồng nhãn và ghi chú cho 10 ảnh train; chưa xác nhận hoàn thành phần học viên tự rà. |

## 8. Công cụ và tài liệu tham khảo

- CVAT: công cụ học viên dùng để gán nhãn.
- Google Colab, Ultralytics YOLO26n và PyTorch: chạy train, validation và prediction theo notebook.
- Tài liệu: [README](../../README.md), [GUIDE](../../GUIDE.md), [CHECKPOINTS](../../CHECKPOINTS.md), [RULES](../../RULES.md), [RUBRIC](../../RUBRIC.md), [SUBMISSION](../../SUBMISSION.md) và notebook bài lab. Không dùng nguồn bên ngoài để truy tìm nhãn.
