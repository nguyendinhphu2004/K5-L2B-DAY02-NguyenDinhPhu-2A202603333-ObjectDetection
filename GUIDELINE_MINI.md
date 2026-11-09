# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp     | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| ------- | ------------------ | -------------------- |
| `car`   | Xe ô tô con chở người cỡ nhỏ: sedan, hatchback, SUV, taxi, xe bán tải/cabin nhỏ. | Thân thấp, ngắn; có cửa sổ dọc hai bên thân và đuôi xe liền thân. Taxi (vàng, đỏ) vẫn là `car`. |
| `truck` | Xe tải chở hàng, xe đầu kéo, xe tải chuyên dụng (xe cứu hộ, xe chở rác/hàng). | To, cao; có cabin riêng và thùng hàng phía sau, thùng không có cửa sổ. |
| `bus`   | Xe buýt, minibus chở khách (kể cả xe buýt nối hai khúc, xe buýt hai tầng). | Rất dài, cao; nhiều cửa sổ thành dãy dọc thân, có cửa lên xuống. |
| `van`   | Xe van, minivan chở hàng hoặc chở người. | Thân hộp, cao và dài hơn `car` nhưng nhỏ hơn `bus`; thường có cửa sổ phía trước, phần thân sau kín hoặc ít cửa sổ. Xe bán tải không thuộc `van` mà thuộc `car`. |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: cạnh box chạm sát phần thân xe nhìn thấy được (từ đầu xe đến đuôi xe, từ nóc đến bánh), không lấy thêm mặt đường, bóng đổ hay xe bên cạnh. Mỗi xe một box riêng, không gộp nhiều xe thành một box.
- Vật thể bị che khuất: vẽ box quanh phần nhìn thấy được nếu đủ để nhận ra lớp; hai xe chồng nhau thì vẽ hai box riêng trên phần nhìn thấy của từng xe. Nếu chỉ còn mảnh nhỏ không xác định được lớp thì không gán và ghi lại.
- Vật thể bị cắt bởi rìa ảnh: box chạm đúng rìa ảnh, không đoán phần nằm ngoài ảnh; vẫn gán nếu nhận ra được lớp.
- Vật thể rất nhỏ/mơ hồ: xe ở xa quá nhỏ hoặc nhòe, không phân biệt được lớp thì không tự đoán; ghi vào bảng bên dưới và hỏi giảng viên. Xe chạy nhòe nhưng vẫn nhận ra được lớp thì vẫn gán.
- Ngoài phạm vi (không gán): mô tô/xe máy, xe ba bánh, xe đạp, người đi bộ, biển báo, đèn giao thông, hình xe trên bảng quảng cáo, phản chiếu trên kính/gương.
- Xe đang đỗ vẫn gán theo loại xe.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | ---------- | --------------- | ----------------- |
| `drive_014` | Xe cứu hộ/kéo xe màu trắng có cần cẩu phía sau, ở giữa nút giao. | `truck` | Là xe tải chuyên dụng có cabin và thùng/cần phía sau, không phải xe con; schema xếp xe tải chở hàng/chuyên dụng vào `truck`. |
| `drive_024` | Xe ba bánh chở người/hàng (kiểu tuk-tuk) ở phía dưới ảnh, cùng nhiều xe máy. | Không gán | Không thuộc bốn lớp `car/truck/bus/van`, tương tự mô tô/xe máy. |
| `drive_030` | Xe MPV/minivan màu tối cỡ lớn ở góc dưới bên phải, cao và dài hơn các sedan xung quanh. | Tạm gán `van`, ghi lại để hỏi giảng viên | Thân hộp, dài hơn xe con; nằm giữa `car` và `van` nên là ca mơ hồ theo GUIDE, không tự chắc chắn. |
| `drive_042` | Xe tải chở hàng chất cao bên cạnh một xe buýt bị che một phần và cắt khung ở mép trái. | `truck` cho xe tải; `bus` cho xe buýt, box quanh phần nhìn thấy | Hai xe chồng nhau nên vẽ hai box riêng trên phần nhìn thấy; phần bị cắt khung chạm đúng rìa ảnh. |
| `drive_012` | Minibus màu trắng/kem nhỏ ở giữa nhiều xe, khó phân biệt giữa `bus` và `van`. | Tạm gán `bus`, ghi lại để hỏi giảng viên | Có dãy cửa sổ dọc thân và chở khách nên theo schema thuộc `bus`/minibus; hình dạng gần `van` nên cần xác nhận. |
