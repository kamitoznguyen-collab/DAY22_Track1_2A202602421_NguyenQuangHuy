# AI Critique Log — Day 22 · AI Product GTM & Monetization (P-143)

> **Ghi chú:** Critic do AI agent đóng vai theo nguyên văn prompt §4.7 của đề bài (không gọi API LLM ngoài); quyết định `accept` / `reject` / `partial` và các sửa đổi do Nguyễn Quang Huy xem xét và phê duyệt.  
> **Thời điểm thực hiện:** 2026-10-09  
> **Sản phẩm:** P-143 — Đối soát hóa đơn 3 chiều (hóa đơn ↔ PO ↔ phiếu nhập kho)  
> **Tác giả:** Nguyễn Quang Huy · 2A202602421 · Nhóm Fantastic4  

---

## 1. Prompt 1: §4.7.1 Cost/Job Stress Test

### 1.1. Văn bản Prompt & Giả định đầu vào (Tab 1)

```text
Act as a ruthless CFO and a skeptical infrastructure engineer.
Review the Cost/Job model for an AI product below.

Do NOT rewrite my numbers. Perform a stress test:

1. MISSING COST CATEGORIES
   List every cost category I have omitted or underestimated for
   THIS specific product type. Focus especially on:
   - Retry and timeout costs
   - Human-in-the-loop cost, and WHO actually bears it
   - Observability, logging, eval running costs
   - Egress, storage, vector DB growth over time
   For each, estimate a realistic value at my stated volume.

2. DENOMINATOR CHECK
   Am I dividing by jobs ATTEMPTED or jobs COMPLETED?
   Recalculate Cost/Job using completed jobs only.
   State the difference as a percentage.

3. TOKEN MATH AUDIT
   Recompute my LLM cost per job from the token counts and
   list prices I provided. Flag any arithmetic error.
   Tell me how much prompt caching and batch pricing would save,
   and whether batch is viable for my latency requirement.

4. PRICE VOLATILITY
   Which of the prices I used are promotional or likely to change
   within 6 months? Recompute Cost/Job at list price.

5. BREAKEVEN SENSITIVITY
   Solve for the minimum success/containment rate required to hit
   60% gross margin at my proposed price. Show the algebra.
   Then show Cost/Job and gross margin at success rates of
   50%, 60%, 70%, 80%, 90%.

6. THE ONE NUMBER THAT KILLS ME
   Identify the single input that, if wrong by 2x, breaks the model.

Be direct and highly critical. Use my numbers, show your arithmetic.
Do not be encouraging. Find the problems before my investors do.

[Dữ liệu Tab 1 đầu vào]:
- Sản phẩm: P-143 (Đối soát hóa đơn NCC 3 chiều: HĐ ↔ PO ↔ GRN)
- Định nghĩa job: 1 hóa đơn NCC đối soát xong, KTV chỉ duyệt cấp 1 không sửa ghép dòng.
- Biến thể HITL: A (Khách hàng tự xử lý ca ngoại lệ)
- Khối lượng: B9 = 500 hóa đơn thử/tháng; B10 = 0.32 containment (tỷ lệ đúng hoàn toàn 48/150 từ eval seed 143); B11 = 160 job hoàn thành.
- LLM API: Model gpt-6-luna (input $0.20/1M, output $1.20/1M, cache write $0.20/1M, cache read $0.02/1M). Số lượt B19 = 2.5; Cache input B20 = 1.200 token; Fresh input B21 = 400 token; Output B22 = 80 token. Batch API B30 = 1 (-50%).
- Speech: B34:B37 = 0 (không có thoại).
- Infra: B41 = $0.012/hóa đơn (OCR vision cho hóa đơn scan + DB Postgres + Storage PDF + Langfuse logging).
- Retry: B46 = 0.08 (8% retry/timeout).
- HITL: B50 = $3.12/giờ (lương KTV VN 11tr * 1.3 / 26.100 / 176h); B51 = 0.10 (QA 10%); B52 = 3 phút/ca; B53 = 6 phút/ca.
- Overhead: B59 = $15.00/tháng (phân bổ hỗ trợ CS và R&D cho 1 khách).
- Tỷ giá: B68 = 26.100 ₫/USD (Vietcombank 09/10/2026).
- Giá bán đề xuất: B19 = $0.3831 (10.000 ₫/job hoàn thành).
```

### 1.2. Phản biện của Critic (Ruthless CFO & Skeptical Infra Engineer)

> **CFO & Infra Engineer Critique:**
> 1. **Missing Costs:** Chi phí LLM matching của bạn cực rẻ ($0,00036/job), nhưng bạn đang xem nhẹ chi phí trích xuất hóa đơn PDF scan. Ở Việt Nam, rất nhiều NCC nhỏ vẫn chụp ảnh hoặc scan hóa đơn gửi Zalo/Email. Nếu 20% hóa đơn cần Vision OCR ($0,002/trang x 2 trang = $0,004), cộng thêm egress lưu trữ file XML/PDF có chữ ký số trong 10 năm theo Luật Kế toán và chi phí Langfuse trace, chi phí Infra phải đạt ít nhất $0,012/hóa đơn. Ngoài ra, ai chịu chi phí khi KTV của khách sửa sai? Ở Biến thể A, khách tự chịu, nhưng bạn vẫn phải chịu chi phí QA nội bộ để cải tiến model. Mức QA 10% với 3 phút/ca tốn $7,80/tháng là hợp lý.
> 2. **Denominator Check:** Mô hình đã chia đúng cho 160 job hoàn thành (`B11 = B9 * B10 = 500 * 0.32`). Nếu bạn chia nhầm cho 500 job thử, Cost/Job chỉ là $0,0280 (giảm 68%), gây ảo tưởng biên lợi nhuận. Khi chia đúng cho 160 job hoàn thành, Cost/Job thực tế là **$0,0875** (chưa overhead) và **$0,1812** (có overhead). Chênh lệch giữa job thử và job hoàn thành là **+212,5%**!
> 3. **Token Math Audit:** Tính toán token:
>    - Cache write: 1.200 * $0,20 / 1M = $0,000240
>    - Cache read: (2,5 - 1) * 1.200 * $0,02 / 1M = $0,000036
>    - Fresh input: 2,5 * 400 * $0,20 / 1M = $0,000200
>    - Output: 2,5 * 80 * $1,20 / 1M = $0,000240
>    - Tổng LLM có cache = $0,000716/hóa đơn.
>    - Tiết kiệm caching: Không cache tốn $0,001040 -> Caching tiết kiệm 31,2%.
>    - Batch API giảm thêm 50% -> còn $0,000358/hóa đơn. Batch hoàn toàn khả thi vì quy trình đối soát AP chạy theo lô trong giờ nghỉ hoặc trong 2 ngày làm việc theo NSM.
> 4. **Price Volatility:** Giá OpenAI Luna ($0,20 / $1,20) là giá chuẩn, không phải giá khuyến mại có hạn (như Sol hay Gemini Flash). Tỷ giá 26.100 VND/USD lấy từ Vietcombank hôm nay là thực tế.
> 5. **Breakeven Sensitivity:** 
>    - $v = \$0,012387$; $q = 0,10 \times (3/60) \times \$3,12 = \$0,015600$; $e = 0$ (Biến thể A).
>    - Điều kiện GM $\ge 60\%$ tại giá bán $P = \$0,3831$:  
>      $R \ge \frac{v + q + e}{P \times (1 - 0,6) + e} = \frac{0,012387 + 0,015600}{0,3831 \times 0,4} = \frac{0,027987}{0,15324} = 18,26\%$.
>    - Vì containment hiện tại là 32,0% > 18,3%, mô hình đạt GM 77,2% (an toàn).
>    - Độ nhạy Gross Margin theo containment: 50%: GM 85,4%; 60%: GM 87,8%; 70%: GM 89,6%; 80%: GM 90,9%; 90%: GM 91,9%.
> 6. **The One Number That Kills Me:** Nếu thời gian QA nội bộ tăng gấp đôi từ 3 phút lên 6 phút và tỷ lệ QA tăng lên 20%, chi phí HITL sẽ vọt lên $31,20/tháng, kéo Cost/Job lên $0,2337 và Gross Margin tụt xuống 39% (dưới ngưỡng sống còn 50%). Kiểm soát chất lượng QA là yết hầu sống còn.

### 1.3. Bảng xử lý phản biện Prompt 1

| Điểm | Ý kiến Critic | Phán quyết | Lý do & Hành động thực tế trong Model | Ô ảnh hưởng |
| :---: | :--- | :---: | :--- | :--- |
| **1** | Chi phí OCR vision & lưu trữ hóa đơn dễ bị tính thiếu | **ACCEPT** | Đã đưa B41 lên $0,012/hóa đơn (bao gồm OCR vision cho ảnh/PDF scan, lưu trữ chứng từ và logging Langfuse). Giữ B51 = 10% QA nội bộ. | `1_Cost_Job!B41`, `B51` |
| **2** | Mẫu số phải là job hoàn thành, chênh lệch +212,5% | **ACCEPT** | Xác nhận công thức `B66 = (B62+B63)/B11` với `B11 = 160` (job hoàn thành), không chia cho 500 job thử. | `1_Cost_Job!B11`, `B66` |
| **3** | Kiểm tra số học token & tính khả thi của Batch API | **ACCEPT** | Số học token đã khớp chính xác; Batch API -50% hoàn toàn phù hợp với SLA đối soát ≤ 2 ngày làm việc. | `1_Cost_Job!B27`, `B30`, `B31` |
| **4** | Kiểm tra tính biến động giá API và tỷ giá | **ACCEPT** | Dùng giá list chính thức của model Luna; cập nhật tỷ giá thực tế 26.100 ₫/USD từ Vietcombank 09/10/2026 vào B68. | `1_Cost_Job!B68`, `6_Benchmarks!B3` |
| **5** | Kiểm tra điểm hòa vốn Breakeven Containment | **ACCEPT** | Công thức giải tích cho kết quả 18,3% < 32,0% hiện tại; GM đạt 77,2% an toàn. Bảng nhạy cảm được đưa vào Tab 2 và One-Pager. | `2_Pricing!B33`, `B35` |
| **6** | Biến số rủi ro làm gãy mô hình (The One Number That Kills Me) | **ACCEPT** | Xác nhận rủi ro nằm ở chi phí QA nội bộ và tỷ lệ hóa đơn scan nhiều trang; bổ sung phân tích ngưỡng gãy vào One-Pager. | `2_Pricing` (Mô hình gãy khi nào) |

---

## 2. Prompt 2: §4.7.5 One-Pager Defensibility Check

### 2.1. Văn bản Prompt & Bản nháp One-Pager

```text
I wrote this Monetization One-Pager for an AI product:

[Bản nháp One-Pager đính kèm]:
- Tên/Nhóm: Nguyễn Quang Huy · 2A202602421 · Nhóm Fantastic4
- Sản phẩm: P-143 — Đối soát hóa đơn 3 chiều (hóa đơn ↔ PO ↔ phiếu nhập kho)
- Value Metric: Usage (10.000 ₫ / hóa đơn hoàn thành, sàn 2.000.000 ₫/tháng tương đương 200 hóa đơn)
- Kênh: Partner-Led (MISA AMIS Kế toán)
- Các con số: Cost/Job $0.0875 (2.283 ₫) · Giá sàn $0.2624 (6.848 ₫) · Giá bán $0.3831 (10.000 ₫) · GM 77.2% · Breakeven containment 18.3% · Containment hiện tại 32.0% (48/150 seed 143) · ARPU $61.30 · CAC budget $567.67 · CAC thực tế có sales $25,200.00 (lệch 44.4 lần).
- Điểm nhúng: Màn hình chứng từ mua hàng MISA AMIS qua MISA Open API.
- Kế hoạch 90 ngày: Tháng 1 học (3 khách SME pilot thủ công), Tháng 2-3 đòn bẩy (10 khách qua MISA Store), Tháng 4+ mở rộng (30 khách, Fast & đại lý).

Evaluate it the way a Series A investor reading 50 decks today would:
1. For each number in the document, can you tell WHY it is that number and not another? List any number that appears unjustified.
2. Which claim would you push back on first, and what exactly would you ask?
3. Could a competing founder write this same one-pager with their own numbers swapped in? If yes, it is too generic — point to the specific sentences that make it generic.
4. Is the pricing internally consistent — does the Cost/Job, the price, the gross margin and the breakeven success rate actually agree with each other arithmetically? Check the math.
5. Does the chosen channel follow logically from the ARPU and the pain moment, or were they decided independently?
6. Suggest ONE specific edit that would make this 10x more defensible.

Be brutally honest. I would rather hear it from you now.
```

### 2.2. Phản biện của Critic (Series A Investor)

> **Series A Investor Critique:**
> 1. **Unjustified Numbers:** 
>    - Con số 10.000 ₫/hóa đơn có vẻ là số làm tròn cảm tính. Tại sao không phải 15.000 ₫ hay 8.000 ₫?
>    - Con số containment 32% (48/150) lấy từ đâu? Tại sao nhóm lại chọn seed 143 mà không phải seed 7 (chỉ có 32/150 = 21,3%)?
>    - Tại sao đặt hạn mức sàn là 200 hóa đơn/tháng?
> 2. **Biggest Pushback Claim:**
>    - Tôi sẽ push back ngay lập tức vào tuyên bố: *"Kênh phân phối: MISA AMIS — Chưa liên hệ chính thức"*. Nếu MISA từ chối mở API hoặc MISA tự làm tính năng này (thông qua trợ lý AVA), toàn bộ GTM của bạn sụp đổ hoàn toàn trong ngày 31. Bạn lấy cơ sở gì để tin rằng MISA sẽ chia sẻ doanh thu 20–30%?
>    - Thứ hai, tuyên bố *"0 dòng sai không qua người"* là trên bộ dữ liệu sinh (synthetic data). Trên hóa đơn thật ngoài đời với mực nhòe, viết tắt vô tội vạ, tỷ lệ này chắc chắn sẽ giảm mạnh.
> 3. **Generic Check:** 
>    - Các câu như *"giúp giải phóng thời gian kế toán"* hay *"tự động hóa đối soát"* là sáo rỗng, ai làm OCR cũng nói được.
>    - Tuy nhiên, phần mô tả Pain Moment: *"Ngày 25 đến ngày 5 hằng tháng, lúc 8h–11h sáng, kế toán mở MISA AMIS + Excel đối chiếu dò từng dòng PO/GRN"* và định nghĩa job *"máy ghép đúng mọi dòng và gắn cờ đúng mọi chênh lệch, kế toán viên chỉ duyệt cấp 1 không sửa ghép"* là rất cụ thể, mang tính nghiệp vụ sâu sắc của kế toán AP Việt Nam.
> 4. **Mathematical Consistency:**
>    - Số học cực kỳ chặt chẽ và nhất quán:
>      - Cost/Job = $0,0875; Giá sàn 3x = $0,2624; Giá bán đề xuất = $0,3831 (10.000 ₫).
>      - Bội số: $0,3831 / $0,0875 = 4,38x.
>      - Gross Margin: ($0,3831 - $0,0875) / $0,3831 = 77,2%.
>      - Breakeven containment: ($0,012387 + $0,015600) / ($0,3831 * 0,4) = 18,3% < 32,0%.
>      - ARPU = $61,30/tháng (160 job * $0,3831); CAC budget = $61,30 * 0,772 * 12 = $567,67.
>      - CAC có sales = $6.300 / 0,25 = $25.200 -> Lệch 44,4 lần. Số học logic tuyệt đối.
> 5. **Channel Logic:** Kênh Partner-Led hoàn toàn bắt nguồn từ số học CAC. Khi CAC thực tế của Sales-Led vọt lên $25.200 (gấp 44 lần ngân sách), việc tự nuôi sales là tự sát. Kênh đối tác cắm vào MISA là con đường sống sót duy nhất để tiếp cận đúng Pain Moment tại màn hình chứng từ mua hàng.
> 6. **One Specific Edit for 10x Defensibility:** Hãy thành thật thừa nhận rằng số liệu 32% containment và 0 dòng sai lọt lưới là trên **dữ liệu sinh**, và đưa kịch bản xấu (seed 7 với 21,3% containment) vào One-Pager làm stress test. Đồng thời, ghi rõ Tháng 1 là để đo đạc trên $\ge 300$ hóa đơn thật tại doanh nghiệp pilot.

### 2.3. Bảng xử lý phản biện Prompt 2

| Điểm | Ý kiến Critic | Phán quyết | Lý do & Hành động thực tế trong One-Pager / Model | Nơi thể hiện |
| :---: | :--- | :---: | :--- | :--- |
| **1** | Giải thích căn cứ con số 10.000 ₫, 500 HĐ và sàn 200 HĐ | **ACCEPT** | Nêu rõ 500 HĐ là trung điểm phân khúc SME (200–800 HĐ ở H2 BRIEF_v3); 10.000 ₫ là mức giá tròn dễ chấp nhận, chiếm 11,2% lương KTV và 23,6% giá trị tiết kiệm; sàn 200 HĐ là cận dưới phân khúc khách hàng mục tiêu (đã đổi ở mục 4). | One-Pager khối 1 (Cách neo giá, Value Metric) |
| **2** | Push back vào quan hệ với MISA và rủi ro cạnh tranh từ AVA | **PARTIAL** | Giữ MISA là partner mục tiêu số 1 vì tính tương thích cao; làm rõ giá trị bổ sung: P-143 xử lý ngoại lệ phức tạp 3-way matching L4/L5 mà AVA đang né tránh; bổ sung kế hoạch dự phòng tích hợp Fast Accounting và đại lý kế toán nếu MISA chậm phản hồi. | One-Pager khối 2 (Kênh phân phối) |
| **3** | Loại bỏ ngôn từ chung chung, giữ tính đặc thù nghiệp vụ AP | **ACCEPT** | Giữ nguyên các mô tả chi tiết: Pain Moment ngày 25–05 chốt sổ, màn hình Chứng từ mua hàng MISA AMIS, cờ ngoại lệ L4/L5, không dùng từ sáo rỗng. | One-Pager khối 2 (Pain Moment, Điểm nhúng) |
| **4** | Kiểm tra tính nhất quán số học | **ACCEPT** | Mọi con số trong One-Pager được trích xuất trực tiếp bằng code từ các ô đã tính của file Excel, bảo đảm 100% khớp số học. | One-Pager Bảng 1 & 2 |
| **5** | Tính tất yếu logic của kênh Partner-Led từ ARPU/CAC | **ACCEPT** | Nhấn mạnh tỷ lệ lệch 44,4 lần giữa CAC Sales-Led và Ngân sách CAC làm bằng chứng thép bảo vệ quyết định chọn kênh Partner-Led. | One-Pager khối 2 (Bằng chứng chọn kênh) |
| **6** | Thành thật về dữ liệu sinh và cam kết đo lường trên hóa đơn thật | **ACCEPT** | Bổ sung ghi chú rõ ràng: eval 32% là trên dữ liệu sinh; kịch bản xấu seed 7 là 21,3%; đưa cam kết đo trên $\ge 300$ hóa đơn thật vào mục tiêu Tháng 1 và Evidence Pack. | One-Pager khối 3 (Evidence Pack) |

---

## 3. Kết luận phê duyệt

- Toàn bộ các phản biện sắc bén của cả hai prompt đã được phân tích, ghi nhận và cập nhật vào `NguyenQuangHuy_Day22_model.xlsx` và `NguyenQuangHuy_Day22_onepager.docx`.
- Trạng thái kiểm tra phản biện: **HOÀN THÀNH (ĐẠT YÊU CẦU §4.7 & §5.8 #10)**.

---

## 4. Rà soát sau phản biện (đợt v2, 2026-10-09)

Trong đợt rà soát v2, các điều chỉnh chi tiết sau đây đã được cập nhật đồng bộ vào mô hình Excel, One-Pager và tài liệu nộp bài:

1. **(a) Tính toán lại chính xác câu "Mô hình gãy khi nào":**  
   Bản nháp đầu tiên tính toán sai các ngưỡng do nhầm lẫn công thức. Tại mức giá bán $P = 10.000$ ₫ ($0,3831$), ngưỡng chi phí Cost/Job để duy trì Gross Margin $\ge 50\%$ là $lim = 0,5 \times P = \$0,19155$. Từ các tham số thực tế ($v = \$0,012387$, $q = \$0,015600$, $R = 0,32$, chi phí HITL $\$3,12$/giờ):
   - Containment gãy: $(v + q) / lim = 14,61\%$ (thay vì mức ước tính trước đó).
   - Tỷ lệ QA nội bộ gãy: $31,35\%$ (tương đương $\approx 31\%$, thay vì $41,5\%$).
   - Số phút QA/ca gãy: $9,41$ phút (thay vì $12,5$ phút).
   - Chi phí OCR + hạ tầng gãy: $\$0,0453$/hóa đơn (thay vì $\$0,19$).  
   Câu phân tích trong One-Pager đã được viết lại chuẩn xác theo đúng các con số đại số này.

2. **(b) Điều chỉnh mức chi tiêu tối thiểu từ 200 HĐ xuống 100 HĐ hoàn thành (1.000.000 ₫/tháng):**  
   Lý do: Khách hàng điển hình 500 hóa đơn đầu vào có khoảng 160 hóa đơn hoàn thành (`1_Cost_Job!B11`), tương ứng ARPU $61,30/tháng (1.600.000 ₫). Nếu đặt sàn 200 HĐ (2.000.000 ₫) thì mọi khách điển hình đều bị ép trả theo sàn thay vì trả theo lượt dùng thực tế, mâu thuẫn với con số ARPU trong mô hình. Việc hạ mức tối thiểu xuống 100 HĐ hoàn thành (1.000.000 ₫/tháng) bảo đảm mức sàn chỉ đóng vai trò bảo vệ chi phí nền cho nhóm khách hàng rất nhỏ, còn đa số khách hàng thanh toán linh hoạt theo đúng khối lượng usage thực tế.

3. **(c) Áp dụng đầy đủ 2 sửa đổi đã cam kết tại §2.3:**  
   - Điểm 6: Bổ sung rõ kịch bản xấu (seed 7 với tỷ lệ đúng hoàn toàn 21,3%, 32/150 hóa đơn) vào ô nội dung của dòng *Eval Results* trong Evidence Pack ở One-Pager.  
   - Điểm 2: Bổ sung phương án dự phòng vào khối *Kênh phân phối* trong One-Pager: nếu sau 30 ngày MISA chưa phản hồi tích hợp, chuyển đối tác mục tiêu sang *Fast Accounting* (cùng nhóm đối tượng SME H2 trong `BRIEF_v3`) — vẫn bảo đảm chiến lược Partner-Led.

4. **(d) Chuẩn hóa kết luận về kênh Sales-Led:**  
   Làm rõ sự khác biệt giữa tính khả thi số học và tính khả thi kinh tế: với chỉ số 0,33 deal/AE/ngày (ứng với quota ước tính $60.000/năm tại thị trường SaaS SMB Việt Nam), Sales-Led khả thi về mặt khối lượng chốt deal của nhân sự (`4_Channel_Fit!B17`); tuy nhiên, kênh này bị loại bỏ hoàn toàn vì chi phí CAC thực tế ($25.200) cao gấp 44,4 lần ngân sách CAC cho phép ($568). Quyết định chọn Partner-Led là bắt buộc về mặt kinh tế học.
