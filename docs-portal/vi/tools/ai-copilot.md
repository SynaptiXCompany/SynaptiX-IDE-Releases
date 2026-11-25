# Trợ lý Kỹ thuật SynaptiX AI Copilot (`Ctrl+Space`)

<div class="doc-meta-bar">
  <span><strong>Mã tài liệu:</strong> SYNX-DOC-TOOL-005</span>
  <span><strong>Phím tắt:</strong> <code>Ctrl+Space</code></span>
  <span><strong>Năng lực:</strong> Tổng hợp Tự động Tag, Luật 4 Tầng, Timer, Modbus &amp; Chuỗi MQTT</span>
  <span><strong>Mô hình An toàn:</strong> Xem trước Đề xuất &bull; Bắt buộc Kỹ sư Nghiệm thu (Human-in-the-Loop)</span>
</div>

**SynaptiX AI Copilot** là trợ lý kỹ thuật tự động hóa công nghiệp được nhúng trực tiếp bên trong SynaptiX IDE. Chỉ cần nhấn `Ctrl+Space` từ bất kỳ màn hình nào để mở bảng trợ lý rộng 380 px bên cạnh, mô tả bài toán điều khiển hoặc thu thập dữ liệu bằng tiếng Việt hoặc tiếng Anh tự nhiên — Copilot sẽ tự động cấp phát biến Tag đúng kiểu vật lý, thiết kế chuỗi luật 4 tầng, khối hẹn giờ/bộ đếm, hàm quy đổi tuyến tính, bộ quét RS-485 Modbus và đường ống đẩy dữ liệu JSON lên MQTT.

![Bảng Trợ lý Kỹ thuật SynaptiX AI Copilot & Tổng hợp Sơ đồ Khối](/images/synaptix_ai_copilot.png)

---

## 1. Thiết lập Ban đầu & Nhận thức Ngữ cảnh Dự án

### 1.1 Cấu hình Khóa API, Nhà cung cấp Đa nền tảng & Bảo vệ Thông tin Xác thực
1. Nhấn `Ctrl+Space` để mở bảng **SynaptiX AI**, sau đó nhấp vào nút bánh răng **[Cài đặt]** ở góc trên.
2. **Lựa chọn Nhà cung cấp AI (Provider):**
   - **Google Gemini:** Chọn mô hình phản hồi tốc độ cao (`gemini-3.8-flash` mặc định, `gemini-3.7-flash` hoặc `gemini-3.6-flash`).
   - **OpenAI & Mô hình Cục bộ tương thích:** Hỗ trợ kết nối với `OpenAI (GPT-4o, GPT-4o-mini)`, `Azure OpenAI` hoặc các máy chủ mô hình cục bộ chuẩn OpenAI endpoint (Ollama, vLLM, LMStudio) phục vụ các nhà máy có yêu cầu cô lập mạng tuyệt đối.
3. **Bảo vệ Thông tin Xác thực bằng DPAPI (Credential Protection):**
   - Khóa API và mật khẩu kết nối được bảo vệ bởi dịch vụ mã hóa Windows DPAPI (`CredentialProtectionService`) với phạm vi `CurrentUser`.
   - Thông tin nhạy cảm luôn được che giấu (`masked`) trên giao diện, không bao giờ xuất hiện trong file log hay bị lưu đính kèm vào tệp dự án `.splc` khi chia sẻ ra ngoài.

### 1.2 Thanh Ngữ cảnh Trực tiếp & Chip Gợi ý Thông minh
Khác biệt hoàn toàn với các công cụ hỏi đáp AI rời rạc trên trình duyệt không hề biết cấu hình phần cứng của bạn, SynaptiX AI Copilot đọc trực tiếp ngữ cảnh dự án thời gian thực:
- **Quỹ Tài nguyên Thiết bị:** Tự động tuân thủ số lượng chân `DI`, `DO`, `AI`, `VREG`, `VREG_RETAIN` và `COUNTER` của [Cấu hình Thiết bị (Device Preset)](/vi/concepts/ecosystem-overview) đang mở.
- **Thanh Trạng thái Ngữ cảnh:** Liên tục theo dõi **Số lượng Khối trên bản vẽ**, **Tổng số Tag**, Trạng thái Biên dịch (bao gồm cả mã lỗi biên dịch hiện hành để AI giải thích và tự sửa lỗi) và **Các khối đang được bôi chọn**.
- **Chip Gợi ý Động (`SuggestionChips`):** Đề xuất các mẫu câu lệnh 1 chạm phù hợp với trạng thái hiện tại của sơ đồ.

---

## 2. Các Hạng mục Mạch Logic AI Copilot Có thể Thiết kế

<div class="synx-node-chain" style="margin-bottom: 12px;">
  <div class="synx-canvas-node is-step">
    <div class="synx-node-header">
      <div class="synx-node-header-left">
        <svg class="synx-node-icon" viewBox="0 0 16 16"><path d="M2,8 L12,8 M8,4 L12,8 L8,12" /></svg>
        <span>BƯỚC 1</span>
      </div>
      <span class="synx-node-status">PROMPT</span>
    </div>
    <div class="synx-node-body">
      <div class="synx-node-pin is-in" style="visibility: hidden;">● In</div>
      <div class="synx-node-content">
        <span class="synx-node-tag">Mô Tả Kỹ Thuật</span>
        <span class="synx-node-alias">Ngôn Ngữ Tự Nhiên</span>
        <span class="synx-node-val">Yêu Cầu Bài Toán</span>
      </div>
      <div class="synx-node-pin is-out">Out ●</div>
    </div>
  </div>

  <div class="synx-node-wire">
    <div class="synx-wire-line"></div>
    <span class="synx-wire-badge">Infer</span>
  </div>

  <div class="synx-canvas-node is-step">
    <div class="synx-node-header">
      <div class="synx-node-header-left">
        <svg class="synx-node-icon" viewBox="0 0 16 16"><path d="M3.5,6 L8,9 L3.5,12 M9.5,7 L13,7 M9.5,10.5 L13,10.5" /></svg>
        <span>BƯỚC 2</span>
      </div>
      <span class="synx-node-status">REASONING</span>
    </div>
    <div class="synx-node-body">
      <div class="synx-node-pin is-in">● In</div>
      <div class="synx-node-content">
        <span class="synx-node-tag">AI Suy Luận</span>
        <span class="synx-node-alias">Ngữ Cảnh &amp; Giới Hạn</span>
        <span class="synx-node-val">Phần Cứng Target</span>
      </div>
      <div class="synx-node-pin is-out">Out ●</div>
    </div>
  </div>

  <div class="synx-node-wire">
    <div class="synx-wire-line"></div>
    <span class="synx-wire-badge">Generate</span>
  </div>

  <div class="synx-canvas-node is-step">
    <div class="synx-node-header">
      <div class="synx-node-header-left">
        <svg class="synx-node-icon" viewBox="0 0 16 16"><path d="M8,1.5 L13.5,4 L13.5,9.5 Q13.5,13.5 8,15 Q2.5,13.5 2.5,9.5 L2.5,4 Z" /></svg>
        <span>BƯỚC 3</span>
      </div>
      <span class="synx-node-status">SYNTHESIS</span>
    </div>
    <div class="synx-node-body">
      <div class="synx-node-pin is-in">● In</div>
      <div class="synx-node-content">
        <span class="synx-node-tag">Sinh Dự Án</span>
        <span class="synx-node-alias">Đồ Thị Logic FBD</span>
        <span class="synx-node-val">Tags • Rules • Blocks</span>
      </div>
      <div class="synx-node-pin is-out">Out ●</div>
    </div>
  </div>

  <div class="synx-node-wire">
    <div class="synx-wire-line"></div>
    <span class="synx-wire-badge">Preview</span>
  </div>

  <div class="synx-canvas-node is-step is-done">
    <div class="synx-node-header">
      <div class="synx-node-header-left">
        <svg class="synx-node-icon" viewBox="0 0 16 16"><path d="M2,8 L4,8 M12,8 L14,8 M4,4.5 A3.5,3.5 0 1 1 4,11.5" /></svg>
        <span>BƯỚC 4</span>
      </div>
      <span class="synx-node-status is-active">● DIFF VIEW</span>
    </div>
    <div class="synx-node-body">
      <div class="synx-node-pin is-in">● In</div>
      <div class="synx-node-content">
        <span class="synx-node-tag">Xem Trước &amp; Duyệt</span>
        <span class="synx-node-alias">So Sánh Trực Quan</span>
        <span class="synx-node-val">Apply • Ctrl+Z Undo</span>
      </div>
      <div class="synx-node-pin is-out" style="visibility: hidden;">Out ●</div>
    </div>
  </div>
</div>

| Phân hệ Tự động hóa | Kết quả Đầu ra Được Sinh tự động |
| :--- | :--- |
| Quy hoạch Danh mục Biến | Cấp phát các ô nhớ `DI`, `DO`, `AI`, `VFLAG`, `VREG` hoặc `VREG_RETAIN` còn trống, gán chuẩn kiểu vật lý `BOOL` / `INT32` / `REAL` kèm tên gợi nhớ (`Alias`) và đơn vị đo. |
| Mạch Khóa Liên động Chuẩn | Xây dựng trọn vẹn đường ống `INPUT -> TRIGGER -> COMPARATOR -> GUARD -> ACTION` với cơ chế tự giữ `Set` / `Reset` và cổng khóa an toàn dừng khẩn cấp E-Stop. |
| Định thời, Đếm, Quy đổi & Toán học | Nối dây bộ trễ đóng [`TON`](/vi/blocks/ton), trễ ngắt [`TOF`](/vi/blocks/tof), phát xung [`TP`](/vi/blocks/tp), đếm sản phẩm [`CTU`](/vi/blocks/ctu), tuyến tính hóa [`SCALE`](/vi/blocks/scale) và bộ số học ALU [`MATH`](/vi/blocks/math). Hỗ trợ kéo dây đa ngõ ra Fan-Out tới nhiều Action đồng thời. |
| Truyền thông RS-485 & Ngoại vi | Khởi tạo khối đọc [`MB_READ`](/vi/blocks/mb-read), ghi [`MB_WRITE`](/vi/blocks/mb-write), khối xuất xung cọc cứng [`DIRECT_WRITE`](/vi/blocks/direct-write) với thông số thanh ghi Modbus chuẩn theo thiết bị ngoại vi thực tế. |
| Điện toán Biên IIoT & MQTT | Khởi tạo khối đóng gói JSON đa biến [`MQTT_PUB`](/vi/blocks/mqtt-pub) và khối nhận lệnh điều khiển [`MQTT_SUB`](/vi/blocks/mqtt-sub) kèm cấu hình topic, QoS và trích xuất trường tự động. |
| Chẩn đoán & Sửa lỗi Biên dịch | Phân tích nguyên nhân lỗi ngữ pháp sơ đồ hoặc lỗi xung đột ghi trùng biến và đề xuất phương án đấu dây chuẩn xác. |

### 2.1 Trí tuệ Ngoại vi Công nghiệp (`Peripheral Intelligence`)
Khác với việc phỏng đoán thanh ghi thông thường, Copilot được tích hợp **Bộ Đăng ký Bản mẫu Ngoại vi (`PeripheralTemplateRegistry`)** chứa cơ sở tri thức phần cứng chuẩn công nghiệp:
- **Đồng hồ Đo Điện năng Thông minh (Smart Energy Meters):** Tự động điền đúng địa chỉ thanh ghi Modbus RTU cho Điện áp pha ($V$), Dòng điện ($A$), Công suất hoạt động ($kW$), Hệ số công suất ($cos\phi$) và Điện năng tiêu thụ ($kWh$) từ các dòng Schneider, Selec, Acrel.
- **Biến tần & Khởi động mềm (VFD / Inverters):** Cấu hình chuẩn thanh ghi Lệnh chạy/dừng (`Control Word`), Tần số đặt (`Frequency Reference`), Tần số phản hồi, Dòng điện động cơ và Mã lỗi từ các dòng Delta, INVT, Yaskawa, Danfoss.
- **Cảm biến Môi trường & Khí tượng:** Bản mẫu trích xuất nhiệt độ RTD/PT100, độ ẩm, áp suất đường ống qua chuẩn Modbus hoặc bộ chuyển đổi tín hiệu tương tự `4..20 mA`.

### 2.2 5 Hình mẫu Kiến trúc Công nghiệp (5 Industrial Archetypes)
Khi phân tích yêu cầu từ người dùng, Copilot tự động nhận diện và áp dụng cấu trúc phù hợp nhất:
1. **Chu trình Tuần tự / Máy trạng thái (State Machine):** Tự động phân rã thành các bước kế tiếp (`Step 1` $\rightarrow$ `Step 2` $\rightarrow$ `Step N`) sử dụng cờ nhớ `VFLAG` hoặc thanh ghi trạng thái `VREG`.
2. **Khóa Liên động & Cảnh báo An toàn (Safety Interlock & Alarm):** Ưu tiên các điều kiện dừng khẩn cấp E-Stop, áp suất tới hạn, quá nhiệt rơ-le và duy trì trạng thái ngắt an toàn Fail-Safe.
3. **Điều tiết Tương tự & Tuyến tính (Analog Regulation):** Kết hợp khối đo lường `AI`, chuẩn hóa `SCALE`, khối tính toán `MATH` và khối so sánh ngưỡng có vùng trễ (Hysteresis) để chống dao động bật/tắt liên tục.
4. **Thu thập & Đẩy Dữ liệu Biên (Edge Telemetry Ingestion):** Tối ưu hóa chu kỳ quét Modbus, gom nhóm thanh ghi liên tục và ánh xạ vào bản tin JSON MQTT gọn nhẹ tiết kiệm băng thông.
5. **Mẻ Sản xuất & Quản lý Công thức (Recipe & Batch Execution):** Quản lý các tham số mẻ trên vùng nhớ Flash lưu giữ bền vững `VREG_RETAIN` không mất dữ liệu khi mất nguồn.

### 2.3 Cơ chế Giao dịch Bản vẽ Nháp (Draft Graph Transactions)
Mọi đề xuất chỉnh sửa hoặc sinh mới từ Copilot đều được bao bọc trong một **Giao dịch Bản vẽ Nháp (Draft Graph Transaction)**:
- Sơ đồ được sinh thử nghiệm và kiểm tra tính toàn vẹn (tránh xung đột trùng tên biến hoặc tràn tài nguyên phần cứng).
- Người dùng có thể đối chiếu trực quan (Diff View) các khối mới/thay đổi trước khi chấp nhận.
- Thao tác áp dụng (`Apply`) được ghi nhận như một bước hoàn tác nguyên tử duy nhất — nhấn `Ctrl+Z` để hủy toàn bộ thay đổi ngay lập tức.

---

## 3. Mẫu Câu lệnh Kỹ thuật Chuẩn Công nghiệp (Prompt Patterns)

Các câu mô tả mơ hồ dễ dẫn tới mạch thiếu điều kiện bảo vệ. Khi viết câu lệnh đặc tả rõ **1) Tên chân Tag & Kiểu tín hiệu**, **2) Ngưỡng tác động & Đơn vị kỹ thuật**, **3) Độ trễ chống nhiễu / Vùng trễ (Hysteresis)** và **4) Khóa an toàn E-Stop**, Copilot sẽ sinh ngay sơ đồ đạt chuẩn tủ điện công nghiệp ngay từ lần đầu:

| Bài toán Điều khiển | Câu lệnh Mơ hồ (Không khuyến nghị) | Câu lệnh Kỹ thuật Chuẩn Công nghiệp (Khuyến nghị) |
| :--- | :--- | :--- |
| Xả Áp suất Quá ngưỡng Có Trễ & Khóa E-Stop | *"Khi áp suất cao quá thì mở van xả."* | *"Quy đổi cảm biến áp suất `AI0` (`4..20 mA`) sang dải `0.0..16.0 bar` lưu vào `VREG0` (`ApSuatLoHoi_Bar`). Nếu `VREG0 >= 12.5 bar` duy trì liên tục `5 giây` (`TON`) thì đóng van xả `DO0`, có khóa an toàn nút dừng khẩn cấp thường đóng `DI2 == 1`. Ngắt van `DO0` khi `VREG0 <= 10.5 bar`."* |
| Trễ Ngắt Quạt Làm mát Sau Khi Dừng Máy | *"Tắt máy xong giữ quạt chạy thêm một lúc."* | *"Mạch khởi động trục chính `DO0` bằng sườn lên nút Start `DI0` và dừng bằng nút Stop `DI1`, khóa liên động rơ-le nhiệt bình thường `DI3 == 1`. Điều khiển quạt làm mát `DO1` qua khối `TOF` để `DO1` bật cùng `DO0` và tiếp tục chạy thêm `15 giây` sau khi `DO0` ngắt."* |
| Quét Đồng hồ RS-485 Đẩy Lên MQTT | *"Đọc nhiệt độ Modbus gửi lên MQTT."* | *"Dùng `MB_READ` trên cổng `RS-485 #1` đọc Slave ID `2`: thanh ghi `40001` (`INT16`, hệ số `0.1`) vào `VREG1` (`NhietDoBuong_C`) và `40002` (`UINT16`, hệ số `0.1`) vào `VREG2` (`DoAmBuong_Pct`) chu kỳ `1000 ms`. Đóng gói cả 2 biến kèm mã thiết bị và dấu thời gian UTC đẩy lên MQTT topic `nhamay/lo1/telemetry` mỗi `10 giây` (`QoS 1`)."* |

::: tip Xem trước Đề xuất & Hoàn tác Nguyên tử (`Ctrl+Z`)
Mọi sơ đồ do Copilot tạo ra đều hiển thị ở chế độ xem trước (`HasPendingAiProposal`) với bố cục dàn trang tự động gọn gàng. Ngay cả sau khi bạn nhấn **Áp dụng** đưa lên [Sơ đồ Logic (`Ctrl+2`)](/vi/workspaces/logic-graph-tab), toàn bộ thao tác chèn đa khối được gom thành một bước lịch sử duy nhất — nhấn `Ctrl+Z` bất kỳ lúc nào để hoàn tác tức thì về trạng thái cũ.
:::

---

## 4. Danh mục Nghiệm thu Bắt buộc Bởi Kỹ sư (`Human-in-the-Loop`)

::: danger Nguyên tắc An toàn Bắt buộc (Chuẩn IEC 61508-3)
Sơ đồ do AI tổng hợp luôn đóng vai trò **bản thiết kế nháp**, tuyệt đối không thay thế trách nhiệm kiểm định của kỹ sư tự động hóa. Bạn **BẮT BUỘC** thực hiện đủ 4 bước nghiệm thu dưới đây trước khi nhấn **Triển khai Nguyên tử (`Ctrl+F5`)** xuống máy móc thực tế!
:::

1. **Rà soát Kiểu Dữ liệu Vật lý & Cực tính Tiếp điểm An toàn (`Ctrl+3`):**
   - Đối chiếu cực tính tiếp điểm thường đóng (`NC`) của nút dừng khẩn cấp E-Stop, rơ-le nhiệt, phao cạn nước trên bản vẽ điện thực tế (`== 1` khi mạch kín an toàn, `== 0` khi tác động hoặc đứt dây)!
   - Kiểm tra kiểu `INT32` so với `REAL` (`Float32`) trong [Danh mục Biến](/vi/workspaces/tag-catalog-tab) để các ngưỡng số thập phân (như `12.5 bar`) không bị ép kiểu mất phần lẻ.
2. **Biên dịch Tất định Tiền kiểm tra (`F7`):**
   - Nhấn `F7` để xác nhận **0 Lỗi / 0 Xung đột Ghi biến** trên thanh trạng thái và trong [Bảng Luật (`Ctrl+4`)](/vi/workspaces/rule-table-tab).
3. **Chạy Mô phỏng Ngoại tuyến Cả Chiều Thuận & Dữ liệu Xấu (`F5`):**
   - Mở [Bộ Mô phỏng Ngoại tuyến (`F5`)](/vi/tools/simulator-and-live-debug) và kiểm chứng đủ **3 kịch bản bắt buộc**:
     - *Luồng Chuẩn (Happy Path):* Chu trình bật/dừng bình thường và tác động đúng sau khi hết thời gian trễ `TON`.
     - *Chống Nhiễu Gai (Boundary):* Xung nhiễu ngắn hơn thời gian `TON.PT` hoặc dao động trong vùng trễ không được phép làm nhảy rơ-le giả.
     - *Giả lập Sự cố (Fault Injection):* Ngắt tiếp điểm E-Stop hoặc khóa `GUARD` phải cắt ngay lập tức cơ cấu chấp hành về trạng thái an toàn (`0`).
4. **Nạp Nguyên tử Hai pha & Giám sát Trực tuyến (`Ctrl+F5` -> `F8`):**
   - Thực hiện nạp qua [Không gian Triển khai (`Ctrl+6`)](/vi/workspaces/deploy-tab) khi đã cách ly tải cơ khí, sau đó bật **Gỡ lỗi Trực tuyến (`F8`)** kiểm tra đúng chiều cảm biến thực tế trước khi đóng điện động lực.

---

## Tài liệu Liên quan

- [Sơ đồ Logic — Logic Graph (`Ctrl+2`)](/vi/workspaces/logic-graph-tab)
- [Thư viện Mẫu — Blueprints (`Ctrl+5`)](/vi/workspaces/blueprints-tab)
- [Mô phỏng Ngoại tuyến & Gỡ lỗi Trực tuyến (`F5` / `F8`)](/vi/tools/simulator-and-live-debug)
- [Kịch bản Tích hợp Thực tế (App Notes)](/vi/tools/application-notes)