# K4 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 4 tiếng
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng `*Câu trả lời của bạn*` bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.5, 1.0 và 1.5 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**
Temperature 0.0 (Thời gian: 2.59s):

"Một sự thật thú vị: Việt Nam là quê hương của hang Sơn Đoòng ở Quảng Bình — hang động tự nhiên lớn nhất thế giới được biết đến hiện nay..." $\rightarrow$ Nhận xét: Model chọn câu trả lời có xác suất xuất hiện cao nhất, kinh điển và an toàn nhất (hang Sơn Đoòng), câu từ gãy gọn, mang tính khuôn mẫu.

Temperature 0.5 (Thời gian: 4.80s):

"Một sự thật thú vị: Việt Nam là nơi có hang động tự nhiên lớn nhất thế giới — hang Sơn Đoòng ở Quảng Bình. Hang này lớn đến mức bên trong có rừng nhiệt đới, sông ngầm, vách đá khổng lồ... UNESCO công nhận..." $\rightarrow$ Nhận xét: Vẫn là chủ đề Sơn Đoòng nhưng lối hành văn mượt mà, tự nhiên và mở rộng thêm nhiều chi tiết phong phú hơn.

Temperature 1.0 (Thời gian: 2.56s):

"Việt Nam có chùa Tam Chúc ở Hà Nam — một trong những quần thể chùa lớn nhất thế giới, với diện tích khoảng 5.100 ha. Nơi đây nổi bật bởi sự kết hợp kiến trúc..." $\rightarrow$ Nhận xét: Model bắt đầu rẽ sang một chủ đề hoàn toàn khác lạ và độc đáo hơn (Chùa Tam Chúc), vốn từ đa dạng hơn ("Vịnh Hạ Long trên cạn").

Temperature 1.5 (Thời gian: 1.96s):

"Một sự thật thú vị: Việt Nam là một trong những nước xuất khẩu cà phê lớn nhất thế giới — đứng thứ 2 toàn cầu sau Brazil. Đặc biệt, cà phê sữa đá là món uống rất mang tính biểu tượng..." $\rightarrow$ Nhận xét: Độ ngẫu nhiên tăng mạnh, model nhảy sang chủ đề khác (văn hóa cà phê).

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
Khi temperature càng tăng, tính ngẫu nhiên (randomness) và độ đa dạng trong việc lựa chọn chủ đề/từ ngữ của mô hình càng lớn. Ở mức thấp (0.0 – 0.5), mô hình tập trung vào các sự thật phổ biến nhất (như hang Sơn Đoòng) với văn phong quy chuẩn và tính nhất quán cao. Khi tăng lên 1.0 – 1.5, mô hình khai thác các token có xác suất thấp hơn để đưa ra các ý tưởng mới lạ (chùa Tam Chúc, văn hóa cà phê), nhưng khả năng dự đoán trước và tính ổn định của phản hồi sẽ giảm dần.

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
Tôi sẽ đặt temperature ở mức thấp, khoảng 0.0 đến 0.2. Vì chatbot hỗ trợ khách hàng đòi hỏi tính chính xác, nhất quán cao và phải bám sát chính sách/tài liệu của doanh nghiệp, tránh tối đa hiện tượng "ảo giác" (bịa đặt thông tin giá cả, quy định). Mức temperature thấp đảm bảo câu trả lời luôn có tính dự đoán cao và ổn định giữa các lần hỏi khác nhau của khách hàng.

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**

Với đơn giá output $0.010 so với $0.0006 / 1K token, GPT-4o đắt hơn GPT-4o-mini khoảng 16.7 lần (tương đương ~$105/ngày so với ~$6.3/ngày cho 10.5 triệu token).

Nên dùng GPT-4o: Khi cần suy luận sâu, chính xác tuyệt đối và xử lý nghiệp vụ phức tạp như thẩm định hợp đồng pháp lý hoặc phân tích tài chính đầu tư.
Nên dùng GPT-4o-mini: Với các tác vụ đơn giản, tần suất cao như phân loại phản hồi khách hàng, trích xuất dữ liệu cơ bản hoặc tóm tắt văn bản ngắn.

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
Hai phản hồi có sự khác biệt rõ rệt: phản hồi của giáo viên tiểu học dùng từ vựng đơn giản, câu ngắn kèm ví dụ trực quan gần gũi (như cuốn sổ ghi chép mượn kẹo), trong khi chuyên gia tài chính sử dụng thuật ngữ chuyên sâu (cơ sở dữ liệu phân tán, sổ cái, hash, cơ chế đồng thuận) với cấu trúc phân tích kỹ thuật chặt chẽ.

System prompt đóng vai trò như "chỉ thị đạo diễn" định hình đối tượng người nghe, persona và phong cách hành văn của mô hình. Qua đó, nó định hướng việc lựa chọn không gian từ vựng và ví dụ minh họa mà không cần phải thay đổi nội dung câu hỏi đầu vào của người dùng.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
Với một đoạn văn tiếng Việt ~130 từ, công thức ước lượng số từ / 0.75 cho ra ~173 token. Đếm bằng tiktoken thực tế cho ra 164 token trên GPT-4o (chênh lệch khoảng 5.4%), nhưng lên tới 294 token trên các model cũ như GPT-4 (chênh lệch tới gần 70%).

Tiếng Việt tốn nhiều token hơn tiếng Anh vì:

Bộ mã hóa BPE được huấn luyện áp đảo bởi dữ liệu tiếng Anh, nên hầu hết từ tiếng Anh nguyên vẹn đều là 1 token.
Các nguyên âm có dấu thanh của tiếng Việt chiếm nhiều byte trong chuẩn UTF-8, khiến tokenizer thường xuyên phải phân tách một từ tiếng Việt thành 2–3 sub-word token hoặc byte token riêng rẽ.
---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
Streaming quan trọng nhất trong các ứng dụng tương tác trực tiếp với người dùng (như chatbot, trợ lý ảo sinh bài viết dài hoặc gợi ý code) vì nó giảm thiểu tối đa thời gian chờ phản hồi đầu tiên (Time To First Token), mang lại cảm giác phản hồi tức thì và cho phép người dùng đọc nội dung ngay khi nó đang được sinh ra. Ngược lại, non-streaming lại phù hợp hơn với các tác vụ xử lý ngầm (backend pipelines, cron jobs hàng loạt), các bài toán yêu cầu cấu trúc dữ liệu hoàn chỉnh (như sinh JSON, function calling) để kiểm tra tính hợp lệ trước khi sử dụng, hoặc khi cần đơn giản hóa hạ tầng mạng, dễ dàng cache và retry mà không phải duy trì kết nối SSE liên tục.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**

So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi thế là kéo giãn thời gian chờ tăng theo cấp số nhân sau mỗi lần lỗi, giúp giảm áp lực request dồn dập và tạo "khoảng thở" đủ lớn để server giải tỏa quá tải và hồi phục trạng thái ổn định.

Nếu hàng nghìn client cùng retry với delay cố định (ví dụ đúng 1 giây), toàn bộ các client này sẽ đồng loạt gửi lại request cùng một thời điểm, gây ra hiện tượng "cơn bão retry" (Retry Storm / Thundering Herd). Đợt sóng lưu lượng tập trung này sẽ tiếp tục đánh sập server vừa chớm hồi phục, biến sự cố tạm thời thành tê liệt toàn hệ thống kéo dài.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**

Tôi chọn persona là: Trợ lý gia sư lập trình Python cho người mới bắt đầu.

System Prompt: "Bạn là trợ lý gia sư Python thân thiện và kiên nhẫn. Luôn trả lời bằng tiếng Việt, giải thích khái niệm ngắn gọn và kèm theo ví dụ code minh họa tối giản (dưới 10 dòng). Khi người dùng gặp lỗi, hãy chỉ ra nguyên nhân và cách sửa thay vì viết lại toàn bộ chương trình."

Giải thích các lựa chọn từ ngữ quan trọng:

1. "Luôn trả lời bằng tiếng Việt": Ràng buộc ngôn ngữ cố định, ngăn model tự động chuyển sang tiếng Anh khi người dùng dán vào các đoạn log lỗi hoặc thuật ngữ lập trình tiếng Anh.
2. "Giải thích ngắn gọn kèm ví dụ code dưới 10 dòng": Giúp người mới học không bị ngợp thông tin, đồng thời kiểm soát lượng token output sinh ra để tối ưu độ trễ (latency) và tiết kiệm chi phí API.
3. "Chỉ ra nguyên nhân và cách sửa thay vì viết lại toàn bộ": Định hướng phương pháp sư phạm mang tính tương tác, giúp người học tự tư duy khắc phục lỗi thay vì chỉ ỷ lại vào việc copy-paste code có sẵn.

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**

Hạn chế lớn nhất: Trợ lý bị giới hạn cứng ở 3 lượt hội thoại gần nhất (history[-6:]) và toàn bộ ngữ cảnh sẽ bị mất khi tắt terminal. Việc cắt tỉa thô sơ này khiến mô hình nhanh chóng quên đi các thông tin quan trọng đã thiết lập ở đầu phiên (như phiên bản Python, hệ điều hành, cấu trúc thư mục dự án của người dùng).

Đề xuất cải thiện: Triển khai cơ chế Tóm tắt ngữ cảnh trượt (Rolling Conversation Summary).

Cách triển khai: Khi độ dài history vượt quá 6 messages, kích hoạt một lời gọi ngầm tới gpt-4o-mini để tóm tắt toàn bộ các lượt chat cũ thành 2–3 câu cô đọng (ghi nhận các ràng buộc và thông tin chính). Đoạn tóm tắt này được lưu vào biến summary và gửi kèm vào đầu prompt (Bối cảnh trước đó: {summary}), kết hợp cùng 2 lượt chat mới nhất. Nhờ đó, trợ lý vừa giữ được mạch logic dài hạn mà vẫn tiết kiệm chi phí token input.

---

## Danh Sách Kiểm Tra Nộp Bài

- [x] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [x] Cả 4 checkpoint pytest đều pass
- [x] Tất cả 9 câu trong file này đã được trả lời
- [x] Đã copy bài làm vào folder `solution/`, push lên fork và dán link trên trang bài Lab ở VLearn trước 23:59 ngày 11/09/2026
