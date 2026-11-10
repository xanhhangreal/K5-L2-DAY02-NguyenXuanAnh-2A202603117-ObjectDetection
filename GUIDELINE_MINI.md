# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` (ID 0) | Xe con, sedan, SUV, taxi và xe bán tải theo schema của bài. | Xe bán tải thuộc `car`, không thuộc `truck`. Không bao gồm xe buýt, xe tải hoặc xe máy. |
| `truck` (ID 1) | Xe tải chở hàng và xe đầu kéo. | Phân biệt với xe bán tải thuộc `car`; không chỉ dựa vào kích thước xe trong ảnh để chọn lớp. |
| `bus` (ID 2) | Xe buýt và minibus chở khách. | Phân biệt với van/minivan thuộc `van`. Nếu không rõ là minibus hay van thì ghi ca mơ hồ và hỏi giảng viên. |
| `van` (ID 3) | Xe van, minivan chở hàng hoặc chở người. | Không bao gồm xe buýt lớn hoặc xe con thông thường. Nếu mơ hồ với `bus` hay `car` thì chưa tự chốt lớp. |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: vẽ hình chữ nhật ôm sát phần xe nhìn thấy được, tránh lấy thêm mặt đường, nền, bóng đổ hoặc xe bên cạnh. Mỗi xe hợp lệ có một box riêng; không gộp nhiều xe vào một box.
- Vật thể bị che khuất: vẫn gán nhãn nếu phần nhìn thấy đủ để nhận diện lớp. Box bao quanh phần nhìn thấy của chính xe đó, không đoán phần bị che. Hai xe chồng lên nhau được gán hai box riêng nếu nhận diện được từng xe.
- Vật thể bị cắt bởi rìa ảnh: gán nhãn nếu nhận diện được lớp; cạnh box dừng đúng rìa ảnh tại nơi xe bị cắt. Không suy đoán phần xe nằm ngoài khung hình.
- Vật thể rất nhỏ/mơ hồ: phóng to để kiểm tra. Nếu chỉ còn mảnh quá nhỏ hoặc chưa đủ căn cứ xác định lớp thì không tự đoán; ghi tên ảnh, vị trí và tình huống vào bảng ca mơ hồ để hỏi giảng viên. Không tự đặt thêm ngưỡng kích thước ngoài hướng dẫn bài.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_002.jpg` | Chiếc xe màu trắng ngay bên trái xe buýt lớn ở giữa ảnh. Người gán nhãn chưa phân biệt được loại xe; ảnh chụp màn hình hiện tại mờ và xe bị che một phần. | Chưa chốt lớp. Cần phóng to ảnh gốc trong CVAT; nếu vẫn chưa xác định được thì hỏi giảng viên và cập nhật quyết định sau khi được giải đáp. | Chưa đủ căn cứ phân biệt `car`, `van` và `truck`. Không tự đoán lớp từ màu sắc hoặc kích thước biểu kiến; xử lý ca mơ hồ theo hướng dẫn bài. |
| `drive_007.jpg` | Chiếc xe ở nửa trên ảnh, trong làn sát bên phải dải phân cách theo hướng nhìn ảnh, chếch phía trên bên trái chiếc xe màu tối ở làn kế bên. Xe có hình ảnh nhỏ và mờ, chưa thấy rõ cấu trúc thân xe và khoang phía sau để xác định lớp. | Chưa chốt lớp. Phóng to ảnh gốc trong CVAT để quan sát thân xe, cabin và khoang phía sau; nếu vẫn chưa xác định được thì hỏi giảng viên và cập nhật quyết định sau khi được giải đáp. | Chưa đủ đặc điểm nhận diện để chọn lớp theo schema. Không tự đoán loại xe chỉ từ kích thước biểu kiến hoặc hình dáng mờ. |
| `drive_009.jpg` | Chiếc xe màu tối ở gần giữa ảnh, ngay dưới xe khách màu trắng và phía sau xe buýt lớn ở tiền cảnh. Xe bị che một phần đáng kể; ranh giới giữa phần xe nhìn thấy và các xe xung quanh khó xác định, dễ vẽ box lấn sang xe khác hoặc suy đoán phần bị khuất. | Cần tự kiểm tra lại trên ảnh gốc: nếu nhận diện được lớp, vẽ box riêng ôm sát phần xe thực sự nhìn thấy, không mở rộng theo phần thân xe bị che. Nếu chưa rõ lớp hoặc ranh giới thì hỏi giảng viên trước khi chốt nhãn. | Theo quy tắc xe bị che khuất, chỉ gán khi đủ căn cứ nhận diện lớp; mỗi xe một box theo phần nhìn thấy, không gộp với xe bên cạnh và không đoán phần bị che. |
