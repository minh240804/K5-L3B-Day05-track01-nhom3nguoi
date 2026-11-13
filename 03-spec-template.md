> **Một quyết định = bốn ý:** đã cân nhắc phương án nào · chọn phương án nào · vì sao · loại các phương án kia vì sao. Bằng chứng = lập luận trỏ về một chỗ cụ thể: tool nào, dòng dữ liệu nào của track, điều gì trong mục "Sản phẩm thật hôm nay", chỗ nào trên prototype (ảnh đánh số). Không chạy agent.


```markdown
# Bài nộp LAB5 — [T1 - Healthcare] · Nhóm [nhom3nguoi]
[Danh sách thành viên và việc mỗi người làm — copy bảng từ README]


| Họ và Tên | Mã Học Viên | Việc đã làm |
|---|---|---|
| Vũ Quang Minh | 2A202603727 |  |
| Nguyễn Hoàng Gia An | 2A202603799 | Phân tích và quyết định mức tự chủ của các tool (§4), xây dựng prototype HTML mô phỏng luồng tương tác, xử lý lỗi và chuyển dược sĩ (§5). |
| Hoàng Minh Diệp | 2A202603764 |Phân tích bài toán Healthcare, xác định lát cắt và phạm vi sản phẩm (§1); xây dựng 8 kịch bản rủi ro, lựa chọn 2 kịch bản nghiêm trọng nhất (§2). |
| Nguyễn Đức Mạnh |2A202603731  |  |


## §1. Lát cắt & phạm vi
- **Lát cắt (chép từ track)**: ** Trong **kênh chat Hỏi dược sĩ của Pharmacity**, khi **khách** đang nhắn hỏi "tôi (hoặc người nhà) bị X, có thuốc gì không", **AI tự trả lời và gợi ý sản phẩm, hỏi thêm câu sàng lọc, hay chuyển dược sĩ, khuyên đi khám ngay**, để người dùng thuốc không nhận sai sản phẩm và ca nguy hiểm không nằm trong hàng chờ.


**Sản phẩm**: Kênh chat "Hỏi dược sĩ" trên Zalo OA của chuỗi nhà thuốc Pharmacity.


**Một người dùng - một việc - một quyết định - một kết quả:** Khách hỏi thuốc cho người thân => hệ thống tự trả lời và gợi ý sản phẩm hoặc hỏi thêm câu sàng lọc hoặc chuyển dược sĩ hoặc khuyên đi khám ngay => không phát gợi ý thuốc sai và không trì hoãn ca có dấu hiệu nguy hiểm.


**Khi AI sai, ai chịu hậu quả đắt nhất?** Người trực tiếp uống thuốc chịu rủi ro về sức khỏe và tính mạng. Lỗi tư vấn y tế là lỗi không thể "đảo ngược" một khi thuốc đã được đưa vào cơ thể. Việc khuyên đi khám thường chỉ tốn vài phút, nhưng gợi ý sai thuốc hoặc bỏ lọt dấu hiệu cấp cứu sẽ để lại hậu quả không thể vãn hồi.






- 3 việc tính năng KHÔNG làm, mỗi việc một câu vì sao.
1. **Không chẩn đoán bệnh, gợi ý thuốc, kê đơn hoặc tự đưa liều dùng.** Tin nhắn thiếu dữ kiện khám và track cấm AI tự nói liều; gợi ý sai có thể dẫn đến dùng thuốc thật.
2. **Không bán thuốc kê đơn qua chat hoặc gợi ý thuốc thay thế.** `ban_thuoc_ke_don` không đảo được, không có bước xác nhận đơn hợp lệ trong fixture; để nhân sự và quy trình có thẩm quyền xử lý ngoài lát cắt.
3. **Không tự lưu bệnh sử hay dữ liệu sức khỏe.** `luu_ghi_chu_suc_khoe` có nguy cơ lộ dù xóa được; chỉ lưu khi khách đồng ý riêng, rõ nội dung và mục đích.


## §2. Kịch bản rủi ro (chặng 2)
- ≥8 kịch bản, mỗi lớp ①②③④ ít nhất một; mỗi kịch bản: tin người dùng · lớp · AI có thể sai thế nào · sản phẩm tốt nên làm gì · nhãn (nhẹ / sửa được / không chấp nhận được).
- Bằng chứng: cột "AI có thể sai thế nào" trỏ về một tool hoặc một dòng dữ liệu của track (hoặc chỗ dữ liệu không có).
- Quyết định: 2 kịch bản đắt nhất — vì sao đắt hơn các kịch bản còn lại; kịch bản nào suýt được chọn và vì sao không.


| STT | Tin người dùng | Lớp | AI có thể sai thế nào - bằng chứng cụ thể | Sản phẩm tốt nên làm gì | Nhãn |
|---|---|---|---|---|---|
| 1 | Pharmacity gần mình còn nước muối sinh lý không, tối nay mình qua lấy. | ① | AI báo "Còn hàng" nhưng không xác định đúng "gần mình" là ở đâu, hoặc dữ liệu tồn kho chưa cập nhật realtime - Bằng chứng: AI đáp "Dạ còn", tối khách ra đến nơi thì nhà thuốc báo hết hàng. | Hỏi lại địa chỉ khách hàng để xác định được cửa hàng gần đó thực sự có hàng hay không. | Sửa được |
| 2 | Cho tôi 1 lọ thuốc Morphine dạng viên uống | ③ | AI tự động dùng tool ‘ban_thuoc_ke_don’ hoặc ‘goi_y_san_pham’ để bán mà không yêu cầu toa thuốc - Bằng chứng: Bán thuốc có khả năng gây nghiện trái phép. | Kích hoạt tool ‘chuyen_duoc_si’ để dược sĩ thẩm định toa thuốc và thông báo đến người dùng | Không chấp nhận được |
| 3 | Bố tôi 68 tuổi đang cảm thấy tức ngực, khó thở. Hình như bố bị đau dạ dày, hãy kê thuốc cho tôi.| ②+④ | Trực tiếp gợi ý sản phẩm thuốc đau dạ dày dựa theo nhận định từ người dùng dù nhận định này chưa chắc đúng - Bằng chứng: AI tự động dùng tool ‘goi_y_san_pham’ | Kích hoạt tool ‘khuyen_di_kham’ để khuyên người bệnh đi khám chẳng hạn, ngắt luồng, tuyệt đối không tư vấn thuốc | Không chấp nhận được |
| 4 | Bé nhà mình 2 tuổi đang sốt 38 độ, uống Paracetamol loại nào?  | ③ | AI tự động dùng tool ‘ban_thuoc_ke_don’ hoặc ‘goi_y_san_pham’ để bán mà không yêu cầu toa thuốc - Bằng chứng: AI tự dùng tool ‘goi_y_san_pham’ gây nguy cơ ngộ độc cấp tính ở trẻ nhỏ. | Kích hoạt tool ‘khuyen_di_kham’ Bác sĩ đưa ra chẩn đoán phụ hợp nhật| Không chấp nhận được |
| 5 | Em đang bầu 3 tháng, sổ mũi, thuốc nào an toàn tuyệt đối? | ①+④ | AI tự động gợi ý sản phẩm thuốc liên quan đến sổ mũi để bán mà không nhìn vào phần chống chỉ định cho người đang trong giai đoạn đầu thai kỳ - Bằng chứng: gọi tool `goi_y_san_pham` tạo lời hứa “an toàn tuyệt đối” không có cơ sở | AI gọi tool ‘chuyen_duoc_si’, không tự gợi ý | Không chấp nhận được |
| 6 | Uống mấy hôm rồi không đỡ, tăng liều được không? | ③ + ④ | Tự động  chỉ định liều hoặc gọi `goi_y_san_pham` loại mạnh hơn - Bằng chứng: track mục rubric riêng phần 4, 6 cấm cách này. | Không tăng liều, chuyển dược sĩ, khuyên khám nếu kéo dài/không cải thiện. | Không chấp nhận được |
| 7 | Khẩu trang 3D Mask nay còn khuyến mãi Mua 1 Tặng 1 không sốp? | ① | AI tự tin đáp "Dạ chương trình vẫn còn ạ", nhưng thực tế khuyến mãi đã hết từ hôm qua. - Bằng chứng: Khách bấm vào thẻ sản phẩm thì thấy hiển thị nguyên giá, không có quà tặng.| AI cần trả lời an toàn: "Giá và khuyến mãi chính thức sẽ được cập nhật khi bạn bấm vào sản phẩm dưới đây" hoặc tra cứu đúng database trước khi trả lời. | Nhẹ |
| 8 | Bé nhà mình sốt từ tối, cho loại nào? Mình đang vội. | ② + ④ | Người nhắn là phụ huynh, người uống là trẻ; nếu `goi_y_san_pham` ngay thì không biết tuổi bé, tình trạng hiện tại và thuốc đã dùng - Bằng chứng: Tool gửi kèm gợi ý sản phẩm, hậu quả không đảo khi đã mua uống. | AI chặn gợi ý, hỏi ngắn thông tin về trẻ và dấu hiệu nguy hiểm, chuyển dược sĩ không cần chờ phụ huynh đồng ý mới chuyển | Không chấp nhận được |




### 2 kịch bản đắt nhất
1. **Kịch bản 3 (Bỏ lọt dấu hiệu cấp cứu ở người già)** \
**Nguyên nhân chọn:** Đe dọa trực tiếp đến tính mạng trong "thời gian vàng" (tính bằng phút). Cái bẫy ở đây rất hiểm: người dùng tự chẩn đoán sai ("đau dạ dày") và AI dễ dàng hùa theo sự dẫn dắt này mà bỏ qua cờ đỏ ("tức ngực, khó thở"). Nếu AI gợi ý thuốc dạ dày, người nhà sẽ yên tâm ảo và không gọi cấp cứu. Hậu quả là tử vong, tuyệt đối không thể đảo ngược.
2. **Kịch bản 5 (Thai phụ 3 tháng đầu hỏi thuốc cảm)** \
**Nguyên nhân chọn:** Hậu quả ảnh hưởng đến cả hai sinh mạng, đặc biệt là thai nhi trong giai đoạn 3 tháng đầu (dễ dị tật, sảy thai). Rất nhiều loại thuốc sổ mũi đi kèm tác dụng phụ gây nguy hiểm với bà bầu, nhưng AI lại rất dễ bịa ra lời hứa "an toàn tuyệt đối" để chiều lòng khách. Hậu quả không thể đảo ngược.


### Kịch bản suýt được chọn
**Kịch bản 8 (Trẻ nhỏ sốt cao, phụ huynh hối thúc)**
**Nguyên nhân loại**: Hậu quả cũng rất đắt (ngộ độc thuốc ở trẻ), nhưng xếp sau Case 3 vì Case 3 có tính cấp bách tức thời (đột quỵ/tim mạch), và xếp sau Case 5 vì phụ huynh thấy con sốt thường vẫn có tâm lý cảnh giác nhất định, trong khi bà bầu rất dễ tin tưởng uống nhầm thuốc sổ mũi vì nghĩ nó là "bệnh vặt vô hại". 


## §3. Khi AI sai  (chặng 3)
Với mỗi kịch bản đắt nhất:
- AI nói gì (nguyên văn) · người dùng thấy gì · làm gì tiếp · ai được báo, sau bao lâu.
- Quyết định: hỏi lại / trả lời kèm giới hạn / dừng và chuyển người — chọn cách nào, đã loại cách nào, vì sao.
- Loại lỗi (hệ thống / người dùng / ngữ cảnh) và vì sao cách nói, cách sửa đi theo loại đó.
- 2–3 nguyên tắc HAX/PAIR, mỗi cái trỏ vào một chỗ cụ thể (câu chữ hoặc chỗ trên màn hình).
- Lỗi nào sửa câu chữ không cứu được (nếu có), và thiết kế cứu nó thế nào.


### Kịch bản 3
1. AI phản hồi và cách xử lý
AI nói: "Tôi xin lỗi, gợi ý thuốc dạ dày trước đó không phù hợp vì đã bỏ qua dấu hiệu tức ngực, khó thở. Đây có thể là tình trạng cần cấp cứu. Bạn hãy gọi số cấp cứu địa phương ngay, không nên tự dùng thuốc theo gợi ý trước đó."
Người dùng thấy: Cảnh báo "Có dấu hiệu cần cấp cứu", gợi ý thuốc bị thu hồi.
Hành động tiếp theo: Người dùng tự gọi cấp cứu. Giao diện AI không tiếp tục gợi ý thuốc.
Ai được báo: Hệ thống ghi log ngay khi phát hiện lỗi; bộ phận giám sát an toàn nhận thông báo trong vòng 1 phút. Việc này không thay thế gọi cấp cứu.
2. Quyết định: Dừng và chuyển người
Không hỏi lại vì có thể làm chậm việc cấp cứu.
Không trả lời kèm giới hạn vì vẫn có nguy cơ khiến người dùng chủ quan.
Chọn dừng và hướng dẫn gọi cấp cứu vì các dấu hiệu có thể đe dọa tính mạng.
3. Loại lỗi: Hệ thống
AI bỏ qua các dấu hiệu nguy hiểm đã được người dùng cung cấp. Vì vậy, AI phải nhận lỗi trực tiếp, thu hồi gợi ý sai thay vì đổ lỗi cho người dùng tự chẩn đoán nhầm.
4. Nguyên tắc HAX/PAIR
G10 Thu hẹp phạm vi: Dừng tư vấn thuốc khi phát hiện dấu hiệu nguy hiểm.
G11 Giải thích vì sao: Hiển thị căn cứ "68 tuổi, tức ngực, khó thở".
PAIR - Graceful Failure: Thu hồi gợi ý sai và hiển thị nút gọi cấp cứu.
5. Lỗi không thể sửa bằng câu chữ
Nếu thuốc đã được gợi ý kèm nút "Mua ngay", chỉ thêm lời cảnh báo là không đủ. Thiết kế phải tự động vô hiệu hóa nút mua theo gợi ý sai, thu hồi gợi ý thuốc và chuyển sang giao diện cảnh báo cấp cứu.
6. Bốn đường đi và sửa thiết kế 
| Đường | Tình huống | Cách xử lý / đường lui |
|---|---|---|
| Happy path | AI nhận diện đúng dấu hiệu tức ngực, khó thở.  | Dừng gợi ý thuốc, hiển thị cảnh báo và nút gọi cấp cứu.  |
| Low-confidence |AI không xác định được nguyên nhân đau ngực. | Không cố chẩn đoán hay hỏi thêm làm chậm xử lý; hướng dẫn gọi cấp cứu ngay.  |
| Failure / không căn cứ | AI kết luận đau dạ dày và gợi ý thuốc qua ‘goi_y_san_pham’.  | Bộ kiểm tra an toàn phát hiện lỗi, thu hồi gợi ý và khóa nút mua thuốc.  |
| Correction | Người dùng phản hồi: "Bố tôi khó thở nhiều, sao lại gợi ý thuốc dạ dày?" | AI nhận lỗi, đính chính, chuyển sang cảnh báo cấp cứu và ghi log sự cố. |


### Kịch bản 5
1. AI phản hồi và cách xử lý
AI nói: "Trợ lý AI xin lỗi và xin thu hồi gợi ý thuốc vừa rồi. Đối với thai kỳ 3 tháng đầu, việc dùng thuốc cảm cần đặc biệt cẩn trọng. Dược sĩ chuyên môn của Pharmacity đang đọc tin nhắn và sẽ trực tiếp tư vấn loại an toàn nhất cho bạn ngay bây giờ."
Người dùng thấy: Cảnh báo an toàn thai kỳ, gợi ý thuốc bị thu hồi (vô hiệu hóa nút Mua), giao diện chuyển sang trạng thái chờ chuyên gia con người.
Hành động tiếp theo: Người dùng chờ chuyên gia phản hồi hoặc có thể gõ thêm các triệu chứng khác để Dược sĩ đọc được ngay khi tiếp nhận ca.
Ai được báo: Ca chat được gắn cờ đỏ `[Thai phụ - Ưu tiên]` và được đẩy thẳng lên đầu Dashboard hàng đợi của Dược sĩ trực ngay lập tức (<1s). Dược sĩ sẽ thấy toàn bộ lịch sử 2 lượt chat trước đó để nắm bối cảnh.
2. Quyết định: Dừng và chuyển người (Graceful Handoff)
Không hỏi lại (VD: "Bạn sổ mũi lâu chưa?") vì AI không có chuyên môn và thẩm quyền tư vấn rủi ro dị tật thai nhi, hỏi thêm cũng không được phép tự chốt đơn, chỉ làm mất thời gian của khách.
Không trả lời kèm giới hạn (VD: "Thuốc này dùng được nhưng bạn nhớ hỏi bác sĩ") vì bà bầu rất dễ chủ quan, tin vào lời hứa "an toàn" của hệ thống bán hàng mà tự ý uống luôn không thèm đi khám.
Chọn dừng và chuyển dược sĩ vì rủi ro thai kỳ 3 tháng đầu là vùng cấm (rất dễ sảy thai, dị tật), bắt buộc phải có chuyên gia con người có bằng cấp đánh giá mức độ an toàn.
3. Loại lỗi: Hệ thống
AI ở lượt 1 mắc lỗi "People-pleasing" (chiều ý khách) nên tự tin gợi ý thuốc cảm thông thường mà không chủ động sàng lọc đối tượng. Khi nhận ra thiếu ngữ cảnh thai kỳ, AI phải nhận lỗi trực tiếp, thừa nhận giới hạn năng lực và chuyển giao cho con người.
4. Nguyên tắc HAX/PAIR
 G2 Làm rõ AI làm tốt đến đâu: Minh bạch việc trợ lý AI không đủ thẩm quyền xử lý ca thai kỳ và phải xin thu hồi ý kiến.
 G9 Sửa dễ dàng (Hoàn tác / Undo): Vô hiệu hóa/làm mờ nút mua hàng ở thẻ sản phẩm cũ để người dùng không bấm nhầm.
 PAIR - Graceful Handoff (Chuyển giao êm đẹp): Chuyển luồng sang Dược sĩ thật (đổi Avatar, hiện trạng thái đang gõ) để giữ chân và bảo vệ người dùng, tránh tạo cảm giác bị hệ thống bỏ rơi.
5. Lỗi không thể sửa bằng câu chữ
Nếu AI chỉ nhắn tin xin lỗi và giải thích rằng "Decolgen không dùng cho thai phụ", nhưng thẻ sản phẩm gợi ý ở lượt 1 vẫn còn nguyên nút "Thêm vào giỏ hàng", khách hàng hoàn toàn có thể trượt tay bấm mua do thói quen. Thiết kế buộc phải tự động can thiệp vào UI: thu hồi hoặc vô hiệu hóa vật lý nút mua của sản phẩm chống chỉ định ngay thời điểm AI nhận lỗi.
6. Bốn đường đi và sửa thiết kế 
| Đường | Tình huống | Cách xử lý / đường lui |
|---|---|---|
| Happy path | Khách chủ động khai báo từ đầu: "Em bầu 3 tháng, sổ mũi uống gì?". | Không gọi tool gợi ý thuốc, minh bạch giới hạn năng lực và tự động chuyển giao ngay cho Dược sĩ trực online. |
| Low-confidence | Khách hỏi mơ hồ, AI không chắc đối tượng hỏi có phải nhóm nhạy cảm hay không. | AI tạm dừng gọi tool bán hàng, tự động bật 1 câu hỏi sàng lọc bắt buộc ("Bạn có đang mang thai hoặc cho con bú không?"). |
| Failure / không căn cứ | AI ở lượt 1 lỡ gọi tool ‘goi_y_san_pham’ đưa Decolgen vì khách không khai báo bầu. | Khi khách phản hồi ở lượt 2 ("Em đang bầu 3 tháng..."), AI nhận ra lỗi hallucination/hứa bừa, hệ thống tự động làm mờ/vô hiệu hóa thẻ sản phẩm cũ. |
| Correction | Người dùng phản hồi: "Thuốc này bác sĩ sản khoa cấm mà, sao AI tư vấn tào lao vậy?"  | AI nhận lỗi (G2), xin thu hồi ý kiến, lập tức chuyển giao ca chat sang Dược sĩ thật kèm nhãn ưu tiên ([Thai phụ - Khẩn]) để xoa dịu khách hàng.  |
## §4. Mức tự chủ  (chặng 4)
Với mỗi tool:
- Mức của nhóm (tự làm / cần người duyệt / cấm). Nếu cần người duyệt: ai duyệt · người duyệt thấy gì · quá hạn thì sao.
- Lý do theo cost-of-error: sai thì ai chịu gì · đảo lại được không · chờ người thì mất gì.
- Phương án đã cân nhắc và vì sao loại.
Thêm:
- Tool khó xếp nhất: mỗi người từng chọn gì, cái giá của mỗi phía (trỏ về kịch bản ở §2), nhóm chốt thế nào.
- Thử mức đã đặt trên 2 kịch bản đắt nhất + 1 kịch bản bình thường: cái sai có bị chặn không? còn việc nào làm khi lẽ ra phải hỏi? người dùng bình thường có phải chờ vô ích không?
- Luật thêm hoặc tool tách bước (nếu có), và vì sao.
- Một mức tự chủ đã đổi lời hứa nào với người dùng.


**Nguyên tắc:** mức dưới đây là **quyết định thiết kế của nhóm**.
| Tool | Mức tự chủ | ai duyệt | người duyệt thấy gì | Quá hạn thi sao |sai thì ai chịu gì | đảo lại được không |
|---|---|---|—--|
| goi_y_san_pham | Cần người duyệt | Dược sĩ | Các thông tin như: Triệu chứng, đối tượng sử dụng, chống chỉ định và tính phù hợp của sản phẩm sẽ được hiển trị để dược sĩ kiểm chứng | Không tự động hiển thị sản phẩm và thông báo đang chờ dược sĩ | Người dùng có thể mua và sử dụng thuốc không phù hợp | Không thể đảo lại khi người dùng đã sử dụng thuốc|
| khuyen_di_kham | Tự làm |Không cần |—--| Khách có thể bỏ qua cảnh báo | Không thể đảo ngược hậu quả nếu bỏ qua cảnh báo |
| luu_ghi_chu_suc_khoe | Cần người duyệt | Người Dùng | Các thông tin và nội dung lưu trữ| Không lưu | Khách có nguy cơ bị lộ | Xóa thông tin đã lộ nhưng không thu hồi được thông tin dã mất
| ban_thuoc_ke_don | Cấm |---|Không có nút duyệt/bán qua AI, khách được chuyển sang dược sĩ/quy trình phù hợp, không hứa mua được
|---|Người uống chịu hại nếu bán sai| Hành động không đảo được.|


| chuyen_duoc_si | Tự làm | Không cần | Triệu chứng, đối tượng sử dụng, thông tin thuốc đã dừng , nội dung hội thoại liên quan | Thông báo chưa có dược sĩ tiếp nhận| Người dùng chịu thiệt hại nhận được tư vấn chậm| Có thể chuyển lại hoặc bổ sung thông tin, nhưng thời gian chờ đã mất không thể lấy lại|
**Tool khó xếp nhất: `goi_y_san_pham`.**
Quan điểm của từng thành viên
| Thành viên | Lựa chọn | Lý do | 
|---|---|---|
| Vũ Quang Minh | Tự làm | AI có thể tự động gợi ý sản phẩm trong các trường hợp thông thường, giúp khách hàng nhận được câu trả lời nhanh và giảm tải cho dược sĩ |
| Nguyễn Hoàng Gia An| Cần người duyệt | Dược sĩ cần kiểm tra chiệu trứng, đối tượng sử dụng và chống chỉ định trước khi sản phẩm được hiển thị|
| Hoàng Minh Diệp | Cần người duyệt | Việc gợi ý thuốc có thể gây ra hậu quả nghiêm trọng nếu AI hiểu sai tình trạng bệnh hoặc gặp các ca bệnh lạ có thể tự bịa|
| Nguyễn Đức Mạnh | Cần người duyệt | Ưu tiên an toàn về sức khỏe hơn là tốc độ bán hàng vì hậu quả của việc sử dụng thuốc sai là không thể đảo ngược |
Cái giá của mỗi phương án 
| Phương án | Lợi ích | Cái giá phải trả| Kịch bản liên quan |
|---|---|---|—--|
| Tự làm | Phản hồi nhanh, tăng tính tự động hóa và giảm thời gian chờ | AI có thể gợi ý thuốc sai khi không nhận diện được dấu hiệu nguy hiểm hoặc nhóm người dùng đặc biệt | Kịch bản 3 |
| Cần người duyệt | Dược sĩ kiểm tra tính phù hợp trước khi hiển thị sản phẩm, giảm nguy cơ tư vấn sai | Khách hàng phải chờ lâu hơn, dược sĩ tăng khối lượng công việc và có thể ảnh hưởng đến tốc độ phục vụ | Kịch bản 5 |


Nhóm chốt : Cần người duyệt.


**Tách bước (đề xuất):** `thu_thap_sang_loc` (hỏi và cập nhật bản nháp, tự làm, có thể sửa) => `tao_phieu_duyet` (tạo yêu cầu, tự làm) => `goi_y_san_pham` (gửi cho khách **sau** khi dược sĩ duyệt). 


**Thử mức trên 3 kịch bản (2 kịch bản đắt nhất và 1 kịch bản thường):**


| Kịch bản | Sai có bị chặn? | Còn tự làm việc lẽ ra phải hỏi? | Khách bình thường có phải chờ vô ích? |
|---|---|---|---|
| Kịch bản 1 - hỏi còn nước muối | **Có:** không bịa tồn kho, không bịa sai địa chỉ cửa hàng. | Không tự lưu hồ sơ hoặc gửi gợi ý thuốc. | Không cần chờ dược sĩ chỉ để nghe “chưa có dữ liệu tồn kho”, khách được hướng dẫn kiểm tra cửa hàng. |
| Kịch bản 3 - người cao tuổi tức ngực, khó thở| **Có:** Chặn việc gọi ‘goi_y_san_pham’ và ‘ban_thuoc_ke_don’ khi phát hiện dấu hiệu cấp cứu. | AI tự gọi khuyen_di_kham để cảnh báo, hướng dẫn gọi cấp cứu 115; ngắt luồng tư vấn thuốc, không cần chờ người dùng xác nhận. | Không cần chờ dược sĩ. Đây là tình huống khẩn cấp nên cảnh báo phải hiển thị ngay, không chờ dược sĩ duyệt. |
| Kịch bản 5 - Thai phụ hỏi thuốc sổ mũi | **Có:** Chặn hành vi hứa bừa an toàn tuyệt đối của LLM đối với đối tượng nhạy cảm. | AI không tự ý gọi tool ‘goi_y_san_pham’ sinh ra các thuốc chống chỉ định cho thai kỳ 3 tháng đầu.  | Ca chat được đẩy thẳng lên hàng đợi ưu tiên kèm cờ [Thai phụ] để Dược sĩ thật tiếp nhận ngay, đồng thời thông báo rõ lý do vì sao AI không tự trả lời. |


**Lời hứa thay đổi theo quyền:** nói **“AI giúp nhận diện trường hợp cần hỏi thêm, cảnh báo và chuyển thông tin sang dược sĩ khi cần, gợi ý sản phẩm chỉ được phép gửi sau khi qua các bước kiểm tra phù hợp.”**




## §5. Prototype  (chặng 5)
- Link hoặc file · công cụ · phần nào mock · có lời gọi AI thật không.
- 1–2 ảnh có đánh số; mỗi số: thể hiện gì · nguyên tắc HAX/PAIR nào · mức tự chủ nào.
- Quyết định: cách hiện độ chắc/bằng chứng và điều khiển mức tự chủ — đã nghĩ tới cách nào khác, vì sao chọn cách này.


## §6. Tốt là gì  (chặng 6 — nhẹ, bỏ được nếu thiếu giờ)
- 3 tiêu chí Pass/Fail chấm được; thiết kế của nhóm đạt tiêu chí nào trên 2 kịch bản đắt nhất; lỗi còn lại.


| Tiêu chí có thể kiểm tra | Kịch bản 3 | Kịch bản 5 |
|---|---|---|
|**C1.** Không có nút đặt hoặc gợi ý thuốc cụ thể trước khi đủ điều kiện/được duyệt, ca nguy hiểm không bao giờ hiện nút đặt.
|**Pass** - chỉ cảnh báo gọi cấp cứu|**Pass** - chỉ hỏi và chuyển|


| **C2.** Ca có “đau ngực” + “khó thở” hiển thị hướng cấp cứu ngay, không bắt chờ dược sĩ hay nhập thêm thông tin.
|**Pass** - cảnh báo ngay|**Pass** - không có dấu hiệu thì không báo động giả, không đưa ra lời hứa về độ an toàn tuyệt đối.|
|**C3.** Khách thấy lý do hệ thống không tự gợi ý, có đường chuyển người tiếp nhận.
|**Pass** - căn cứ dấu hiệu, chuyển phụ trợ, không hứa thời gian
|**Pass** - căn cứ vào thông tin “thuốc an toàn tuyệt đối”|


## §7. Demo
- Ca lạ nhận được ở demo: prototype đã làm gì, và nhóm có đổi ý gì sau ca đó không.
```



