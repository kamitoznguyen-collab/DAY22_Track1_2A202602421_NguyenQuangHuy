# Kịch bản demo bằng dữ liệu thật — chi nhánh Nha Trang

> **Dữ liệu nội bộ.** Chỉ chạy trên máy này. Không commit, không gửi file đi đâu. Ngoại lệ duy nhất (đã đồng ý ngày
> 08/10): ảnh hóa đơn scan được gửi tới OpenRouter (Google Gemini) để đọc.
> Code chạy là nhánh `demo/2026-10-09` ở `D:\P-143-demo2`. DB riêng `p143_that`.
> Công ty mua: **CHI NHÁNH NHA TRANG - CÔNG TY CỔ PHẦN VINPEARL**, MST `4200456848-005`.

## Thư mục dữ liệu `D:\P143-du-lieu-that\demo-nha-trang\`

| Thư mục / file | Là gì | Dùng ở bước |
|---|---|---|
| `json\` | 31 PO + 31 phiếu nhập (62 file), dựng từ bộ `po-grn` | 2 |
| `hoa-don-demo\` | **8 hóa đơn chọn sẵn**, mỗi nhà cung cấp một thư mục con | 3 |
| `hoa-don-dai\` | **3 hóa đơn dài**: 108, 88, 47 dòng | 9 |
| `hoa-don\` | Cả 31 hóa đơn (không cần cho demo) | — |
| `BANG-KE.csv` | Hóa đơn ↔ PO ↔ phiếu nhập, kèm lỗi cài sẵn trong phiếu nhập | tra cứu |

## 0. Chuẩn bị (10 phút trước giờ)

1. Mở **Docker Desktop**.
2. PowerShell, chạy lần lượt:
   ```powershell
   powershell -File D:\P143-du-lieu-that\demo-that.ps1 reset
   powershell -File D:\P143-du-lieu-that\demo-that.ps1 start
   ```
   - `reset` xóa sạch dữ liệu cũ và tạo 2 tài khoản.
   - `start` bật backend và giao diện ở cổng 8000/5173, rồi mở trình duyệt. Demo cũ (`D:\P-143`) bị tắt.
   - Lệnh in ra mật khẩu. Ghi ra giấy, không chiếu lên màn hình.
3. Mở sẵn cửa sổ Explorer ở `D:\P143-du-lieu-that\demo-nha-trang`.
4. **Chi phí AI mỗi lượt chạy đủ kịch bản:** khoảng **0,30 USD (≈ 8.000 đ)**. 8 hóa đơn khoảng 0,13 USD, 3 hóa đơn dài
   khoảng 0,15 USD.
5. **Lưu ý: AI đọc mỗi lần có thể hơi khác.** Kết quả bên dưới là của lần chạy ngày 08/10. Một ô lần trước đọc sai, lần
   này có thể đọc đúng, và ngược lại. Nên **chạy tập một lượt** (reset → làm hết) trước buổi demo, rồi `reset` lại.

## 1. Đăng nhập

Đăng nhập **Ngọc** (`ngoc.nguyen@xex.vn`, kế toán viên). Góc trên ghi tên chi nhánh Nha Trang.

## 2. Nạp PO + phiếu nhập (JSON) — làm **trước** khi thả hóa đơn

- **Làm:** menu **Tải lên** → khối **Dữ liệu đối chiếu (PO / Phiếu nhập)** → **Chọn file JSON** → vào `json\` →
  Ctrl+A chọn hết 62 file → Open.
- **Thấy:** "Tạo 31 PO, cập nhật 0; tạo 31 Phiếu nhập, cập nhật 0; 0 lỗi".
- **Nói:** "Đơn hàng và phiếu nhập kho của chi nhánh, xuất từ ERP dạng JSON. Hệ thống nạp PO trước, phiếu nhập sau."

> Thả hóa đơn trước khi nạp PO thì mọi hóa đơn báo đỏ "không tìm thấy PO". Lỡ làm thì `reset` lại từ đầu.

## 3. Thả cả thư mục hóa đơn

- **Làm:** cùng trang Tải lên → kéo **thư mục** `hoa-don-demo` thả vào ô "Kéo thả hóa đơn vào đây" (hoặc bấm **Chọn
  thư mục** → chọn `hoa-don-demo`).
- **Thấy:** một lô 8 file. Mỗi file chạy qua 4 bước Tải lên → Đọc file → Đối chiếu → Xếp loại. **Mất khoảng 1–2 phút**:
  mỗi trang được AI đọc 2 lần.
- **Nói (trong lúc chờ):** "Thư mục gồm nhiều nhà cung cấp, mỗi nhà cung cấp một thư mục con. Hệ thống tự lấy hết hóa
  đơn bên trong. Đây là bản scan, không có lớp chữ, nên AI đọc ảnh 2 lần độc lập rồi so với nhau."

## 4. Kết quả mong đợi (lần chạy 08/10) và thứ tự trình bày

| # | Hóa đơn (số) | Nhà cung cấp | Kết quả | Cảnh để nói |
|---|---|---|---|---|
| A | `00010953` | CN CT TNHH ANGST Trường Vinh (101) | 🟢 Khớp | Hóa đơn sạch: PO, phiếu nhập, giá, thuế đều khớp. Bước 5 |
| B | `00001289` | CT TNHH SX TM Bách Ngân (114) | 🔴 `DOC-01` | **AI đọc sai MST** → sửa cạnh ảnh gốc. Bước 6 |
| C | `00001735` | CT TNHH Minh Khang NT (601) | 🔴 `QTY-01` | **Lỗi cài sẵn:** kho nhận thiếu. Bước 7 |
| D | `534` | CT TNHH TM XD Lê Hoàn Vũ (604) | 🔴 `FRD-05` | **Số tài khoản khác** tài khoản đã lưu. Bước 8 |
| E | `00011397` | CN CT TNHH ANGST (103) | 🟡 `INT-01` | 2 lần đọc lệch nhau → người xác nhận. Bước 6 |
| F | `7164` | CT TNHH Thực phẩm Ân Nam (105) | 🟡 `ITM-02` | Tên hàng gần giống ("Fan 1L" / "Fan1L") → người xác nhận ghép dòng |
| G | `7033` | CT TNHH Thực phẩm Ân Nam (104) | 🟢 Khớp | |
| H | `00013634` | CTY CP Văn Lang (637) | 🟢 Khớp | |

> Ngày 08/10, hóa đơn C và D bị thêm hàng loạt `TAX-01` "KKKNT so với 0%". Nguyên nhân là PO dựng sai thuế suất, **đã
> sửa** trong `json\`: PO giờ ghi KKKNT. Lượt tập tới, C và D chỉ còn `QTY-01` và `FRD-05`. **Chưa chạy lại để xác
> nhận.**

## 5. Hóa đơn khớp (A)

**Hóa đơn** → mở `00010953` → tab **So sánh 3 chiều**. Bên trái là từng dòng hóa đơn cạnh dòng PO và phiếu nhập. Bên
phải là **ảnh hóa đơn gốc**.

## 6. AI đọc sai → sửa cạnh ảnh gốc (B, E) — cảnh mentor muốn

- **B `00001289`:** tab Ngoại lệ ghi "Hóa đơn không ghi số PO; nhà cung cấp chưa có trong danh mục…".
  - **Làm:** tab **So sánh 3 chiều** → bấm ô **MST người bán** (đang là `1801686578`) → nhìn ảnh gốc bên phải → sửa
    thành **`1801686576`** → **Lưu giá trị mới**.
  - **Nói:** "AI đọc nhầm số 6 thành 8 ở cuối MST. Chữ số cuối của MST là số kiểm tra: `…578` sai, `…576` đúng. Sửa
    xong, hệ thống tìm được nhà cung cấp và PO, rồi đối chiếu lại."
  - **Thấy:** hóa đơn tìm được `PO-AUTO-0011` và hết `DOC-01`.
  - **Chưa thử trên giao diện.** Nếu không tự đối chiếu lại thì bấm **Chạy lại**.
- **E `00011397` (🟡):**
  - Các ô có dấu ⚠ là chỗ 2 lần đọc cho số khác nhau.
  - **Làm:** bấm từng ô ⚠ → so với ảnh → **Xác nhận đúng** (hoặc sửa).
  - **Nói:** "Máy không chắc thì không tự cho qua; người xác nhận, có ghi lại ai xác nhận lúc nào."

## 7. Kho nhận thiếu (C, lỗi cài sẵn)

Mở `00001735` → **Ngoại lệ**: `QTY-01` dòng 11 "Thịt bò xay": hóa đơn tính 15 kg, phiếu nhập `GRN-AUTO-0090` chỉ nhận
13 kg, vượt 2 kg.

**Nói:** "Đây là lỗi cài sẵn trong dữ liệu kho. Hệ thống không cho trả tiền 2 kg chưa nhận."

## 8. Số tài khoản khác (D)

Mở `534` → **Ngoại lệ**: `FRD-05` "Hóa đơn ghi tài khoản nhận tiền 7686811111, khác tài khoản đã lưu 76868111111".

**Nói:** "Lệch đúng một chữ số 1. Có thể là AI đọc sót, có thể là thật. Hệ thống chặn và yêu cầu gọi điện xác minh với
nhà cung cấp. Đây là kiểu lừa đảo phổ biến nhất."

Nếu nhìn ảnh gốc thấy đúng là 11 số, sửa ô tài khoản → hết cảnh báo.

## 9. Hóa đơn dài

- **Làm:** **Tải lên** → thả **thư mục** `hoa-don-dai` (3 file).
- **Thời gian:** khoảng 1–2 phút. Hóa đơn 108 dòng có 5 trang, mỗi trang đọc 2 lần.

| Số | Nhà cung cấp | Dòng / trang | Kết quả 08/10 |
|---|---|---|---|
| `386` | VP 4201669084 (C25MHH386) | **108 dòng / 5 trang** | 🟡 `INT-01`, `ITM-02` |
| `244` | 4201669084 (C26MHH244) | 88 dòng / 4 trang | 🟡 `INT-01`, `ITM-02` (phiếu nhập cài sẵn "nhận thừa" dòng 88: không ảnh hưởng thanh toán nên không báo) |
| `00000869` | CT TNHH Nguyên Phương (639) | 47 dòng / 3 trang | 🔴 nhiều mã: `PRC-01/02`, `QTY-01/04/05`, `ITM-02/04` |

- **Nói:** "Hóa đơn hơn 100 dòng, nhiều trang. Hệ thống đọc từng trang rồi ghép lại, rồi ghép từng dòng với PO."
- Mở `386` → **So sánh 3 chiều** → cuộn cho thấy đủ 108 dòng.

## 10. Duyệt 2 cấp

- Ngọc: `00010953` → **Duyệt cấp 1**.
- Đăng xuất → **Hà** (`ha.tran@xex.vn`) → **Chờ duyệt** → **Duyệt cấp 2**.

Nhà cung cấp mới (3 hóa đơn đầu) hoặc hóa đơn trên 50 triệu đều cần 2 cấp.

## Sự cố

| Sự cố | Xử lý |
|---|---|
| Đăng nhập báo sai | Chưa `reset`. Chạy `demo-that.ps1 reset` rồi `start` |
| Hóa đơn đứng "Đang đọc file" quá 3 phút | F5. Vẫn đứng thì mở hóa đơn → **Chạy lại**. Ngày 08/10 có 1 file (`VP_…C26MHH23`) đứng ở bước tải lên |
| Một file báo "Không đọc được số hóa đơn, MST bên bán hay tổng tiền" | Ảnh quá mờ (ngày 08/10: file `632_…`). Bỏ qua, hoặc vào **Nhập tay** |
| Hóa đơn báo trùng `FRD-01` | Đã tải hóa đơn này rồi: đúng thiết kế (chặn trả tiền 2 lần) |
| Kết quả khác bảng ở bước 4 | AI đọc mỗi lần hơi khác. Dùng chính hóa đơn đó để nói về sửa / xác nhận |
| Muốn quay về demo cũ (dữ liệu giả) | `demo-that.ps1 stop`, rồi `powershell -File D:\P-143\demo.ps1 start` |

Sau buổi demo: `powershell -File D:\P143-du-lieu-that\demo-that.ps1 stop`.
