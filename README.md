# Day 22 · AI Product GTM & Monetization Model (P-143)

- **Học viên:** Nguyễn Quang Huy  
- **Mã học viên:** 2A202602421  
- **Nhóm dự án:** Fantastic4  
- **Sản phẩm lựa chọn:** P-143 — Hệ thống AI đối soát hóa đơn 3 chiều (Hóa đơn mua vào ↔ PO ↔ Phiếu nhập kho)  
- **Ngày thực hiện & kiểm tra giá:** 09/10/2026  

---

## 1. Danh mục tài sản nộp bài

| STT | Tên file | Định dạng | Mô tả nội dung |
| :-: | :--- | :---: | :--- |
| 1 | `NguyenQuangHuy_Day22_model.xlsx` | Excel (.xlsx) | Mô hình tài chính và GTM hoàn chỉnh 7 sheet; chỉ điền ô vàng, giữ nguyên toàn bộ công thức gốc và định dạng chuẩn. |
| 2 | `NguyenQuangHuy_Day22_onepager.docx` | Word (.docx) | Tài liệu tóm tắt Monetization One-Pager 3 khối; toàn bộ 11 chỉ số truy xuất chính xác từ mô hình Excel. |
| 3 | `NguyenQuangHuy_Day22_onepager.pdf` | PDF (.pdf) | Bản xuất PDF chuẩn 2 trang từ Word COM, định dạng tinh gọn phục vụ thẩm định và phản biện. |
| 4 | `ai-critique-log.md` | Markdown (.md) | Nhật ký phản biện chi tiết qua 2 prompt chuyên sâu (§4.7.1 và §4.7.5) kèm bảng phán quyết accept/reject/partial. |
| 5 | `README.md` | Markdown (.md) | Tổng quan bài làm, các chỉ số then chốt và hướng dẫn nộp bài. |

---

## 2. Các chỉ số tài chính & GTM cốt lõi

| Chỉ số | Giá trị ($ USD) | Quy đổi (VNĐ) | Tham chiếu ô Excel | Đánh giá & Trạng thái |
| :--- | :---: | :---: | :---: | :--- |
| **Cost / Job** (chưa overhead) | **$0.0875** | 2.283 ₫ | `2_Pricing!B5` | Mẫu số tính trên job hoàn thành (160 job/tháng) |
| **Giá sàn** (3× Cost/Job) | **$0.2624** | 6.848 ₫ | `2_Pricing!B7` | Ngưỡng an toàn tối thiểu theo đề bài |
| **Giá bán đề xuất** | **$0.3831** | **10.000 ₫** | `2_Pricing!B19` | 4,38× Cost/Job; mức giá tròn dễ mua cho SME |
| **Gross Margin** | **77.2%** | — | `2_Pricing!B21` | **OK — AN TOÀN** (vùng 60% – 85%) |
| **Breakeven Containment** | **18.3%** | — | `2_Pricing!B33` | **ĐẠT** (nhỏ hơn nhiều so với containment 32.0%) |
| **ARPU / tháng** | **$61.30** | 1.600.000 ₫ | `4_Channel_Fit!B5` | Tính trên 160 job hoàn thành; mức tối thiểu 100 HĐ = 1,0 tr ₫ |
| **Ngân sách CAC cho phép** | **$567.67** | 14.816.000 ₫ | `4_Channel_Fit!B9` | Payback 12 tháng phân khúc SMB |
| **CAC thực tế Sales-Led** | **$25,200.00** | 657.720.000 ₫ | `4_Channel_Fit!B22` | Theo benchmark ICONIQ 2026 (SMB $6.300 / win 25%) |
| **Bội số lệch CAC** | **44.4 lần** | — | `4_Channel_Fit!B23` | **Sales-Led không khả thi về CAC** (số deal 0,33/AE/ngày vẫn khả thi) |
| **Kênh phân phối 90 ngày đầu** | **Partner-Led** | — | `4_Channel_Fit!B38` | **MISA AMIS Kế toán** (MISA Open API, chia sẻ 20–30%) |

---

## 3. Ghi chú phương pháp luận & Nguồn số liệu

1. **Định nghĩa Job giá trị:** 1 hóa đơn NCC được đối soát 3 chiều xong (HĐ ↔ PO ↔ GRN), máy ghép đúng mọi dòng và gắn cờ ngoại lệ chính xác, Kế toán viên duyệt cấp 1 mà không phải sửa ghép dòng nào.
2. **Biến thể HITL:** Chọn **Biến thể A** (bán phần mềm, KTV của khách tự duyệt & xử lý ngoại lệ theo nguyên tắc 1 của `BRIEF_v3`). Chi phí QA nội bộ 10% được tính đầy đủ vào COGS để bảo đảm chất lượng mô hình.
3. **Nguồn số liệu thực nghiệm:**
   - Tỷ lệ đúng hoàn toàn 32,0% (48/150 hóa đơn) lấy từ kết quả eval thật của P-143 ngày 02/10/2026 (seed 143, model `gpt-6-luna`).
   - *Tính minh bạch:* Số liệu eval là trên dữ liệu sinh (synthetic data). Kế hoạch Tháng 1 trong 90-day plan tập trung onboard thủ công 3 khách pilot dùng MISA để đo lường lại trên $\ge 300$ hóa đơn thật.
4. **Nguồn giá API & Tỷ giá:**
   - Tra cứu ngày 09/10/2026: Tỷ giá Vietcombank bán ra niêm yết **26.100 VND/USD**.
   - Model `gpt-6-luna`: Input $0.20/1M, Output $1.20/1M, Cached read $0.02/1M (giảm 90%), Batch API giảm 50% (đối soát chạy lô bất đồng bộ).
5. **Nguồn các giả định ước tính:**
   - Quota AE $60.000/năm là ước tính cho thị trường SaaS SMB tại Việt Nam; overhead $15/tháng/khách là ước tính phân bổ hỗ trợ CS + R&D.
   - Token LLM/hóa đơn ở tab 1 là giả định thận trọng (≈ $0,00072/HĐ trước batch, cao hơn mức đo $0,0357/150 HĐ ≈ $0,00024/HĐ).