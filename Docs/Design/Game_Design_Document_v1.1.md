# Game Design Document v1.1 — Dark Fantasy Side-Scrolling Party Action RPG

| Trường | Giá trị |
|---|---|
| Phiên bản | 1.1 (design gate) |
| Ngày | 2026-10-22 |
| Thay thế | GDD v1 (draft 2026-10-21, chỉ tồn tại trong phiên thảo luận, chưa từng nằm trong repo) |
| Trạng thái | Draft để phê duyệt — **chưa cấp phép triển khai** |
| Project | D:\Unity Project\NewUnityProject |
| Engine | Unity 6000.6.5f1 (Unity 6.6) |
| Phạm vi phase này | Tài liệu thiết kế + tiêu chí kiểm chứng. Không code, không scene, không prefab, không package. |

## 0. Cách đọc tài liệu này

Mọi phát biểu trong tài liệu được gắn một trong bốn nhãn:

- **[CONFIRMED]** — quyết định của người dùng, không mở lại trong phase này.
- **[DEFAULT-PROTOTYPE]** — giá trị mặc định để prototype chạy và để đo; **không phải** quyết định gameplay cuối cùng, phải bị thay đổi nếu bằng chứng phản bác.
- **[OPEN]** — câu hỏi chưa có câu trả lời; cần bằng chứng prototype hoặc cần người dùng phê duyệt.
- **[FACT-AUDIT]** — quan sát từ file dự án tại thời điểm ghi tài liệu, không phải kết quả chạy Unity.

Nguyên tắc: không có [OPEN] nào được tự động biến thành quyết định cuối bởi việc triển khai prototype. Prototype tồn tại để tạo bằng chứng cho các mục [OPEN].

## 1. Tầm nhìn sản phẩm và quyết định đã xác nhận

### 1.1 Tầm nhìn

Action RPG fantasy side-scrolling, nơi khám phá và cốt truyện ngang hàng với chiến đấu. Người chơi điều khiển trực tiếp một nhân vật tại một thời điểm, xây dựng đội hình bốn người và chuyển đổi nhân vật trong chiến đấu thời gian thực. Sưu tầm nhân vật là động lực dài hạn, nhưng gameplay cốt lõi phải hấp dẫn ngay cả khi người chơi chưa sở hữu nhiều nhân vật.

Hướng nghệ thuật mục tiêu (giữ nguyên từ v1, chưa sản xuất ở phase này): painterly pixel art, stylized dark fantasy, môi trường side-scrolling nhiều lớp; vùng u ám dùng bóng tối sâu, sương mù, ánh sáng điện ảnh, palette trầm; thành trì/thị trấn/khu an toàn được phép sáng và giàu tính fantasy truyền thống hơn.

Octopath Traveler là tham chiếu cho cách trình bày, khám phá, kể chuyện và tiến trình RPG. Đây **không** phải mục tiêu sao chép hệ thống chiến đấu theo lượt.

### 1.2 Quyết định đã xác nhận

| ID | Quyết định | Nhãn |
|---|---|---|
| C-01 | Đội hình chiến đấu bốn slot | [CONFIRMED] |
| C-02 | Người chơi điều khiển một nhân vật tại một thời điểm | [CONFIRMED] |
| C-03 | Chuyển đổi nhân vật trong chiến đấu thời gian thực | [CONFIRMED] |
| C-04 | Trong khám phá chỉ nhân vật đang điều khiển là actor vật lý trên màn hình | [CONFIRMED] |
| C-05 | Single-player trước; local save và cloud save | [CONFIRMED] |
| C-06 | Supabase Free cho prototype backend | [CONFIRMED] |
| C-07 | PC và Android là nền tảng ưu tiên; iOS có thể bổ sung sau | [CONFIRMED] |
| C-08 | Online bất đồng bộ có thể bổ sung sau; multiplayer thời gian thực **không** nằm trong phạm vi | [CONFIRMED] |
| C-09 | Gacha banner là mục tiêu tiến trình dài hạn | [CONFIRMED] |
| C-10 | Roster dài hạn khoảng 20 nhân vật điều khiển được, có thể mở rộng | [CONFIRMED] |

### 1.3 Non-goals của phase này

Không sản xuất art, không thiết kế kinh tế, không chốt gacha, không dựng backend, không chọn hướng combat cuối cùng, không triển khai gameplay.

## 2. Vòng lặp gameplay cốt lõi

### 2.1 Vòng lặp chính

Khám phá khu vực → chiến đấu → giải câu đố/hoàn thành nhiệm vụ → nhận tài nguyên → phát triển nhân vật và đội hình → đối đầu boss → mở rộng khả năng khám phá.

### 2.2 Ba câu hỏi prototype phải trả lời

1. Chuyển đổi giữa bốn nhân vật có tạo đủ chiều sâu chiến đấu để biện minh cho cơ chế này không?
2. Nhịp và cơ chế điều khiển nào phù hợp hơn với side-scrolling?
3. Cấu trúc party và character state có mở rộng từ 4 lên khoảng 20 nhân vật mà không phải xây lại không?

### 2.3 Non-goals của prototype

Không inventory/equipment hoàn chỉnh; không gacha/banner/pity/IAP; không world map; không quest framework; không economy system; không Supabase/cloud save/multiplayer trong bản combat; không bộ asset 20 nhân vật; không art production cho thế giới.

Nguyên tắc giữ nguyên từ v1: không dựng framework cho quest, inventory, world map hay economy trước khi một vòng lặp nhỏ chứng minh các hệ thống đó cần tổ chức ra sao.

## 3. Mô hình bốn nhân vật, một actor hoạt động

### 3.1 Ngữ nghĩa actor và state

- **[DEFAULT-PROTOTYPE]** Trong prototype, tồn tại **đúng một actor vật lý điều khiển được** tại mọi thời điểm. Actor này bền (persistent), không bị destroy/tạo lại mỗi lần chuyển đổi.
- **[DEFAULT-PROTOTYPE]** Nhân vật không hoạt động **không có GameObject, không có collider, không có transform**. Chúng tồn tại dưới dạng runtime state thuần dữ liệu: HP, cooldown từng ability, trạng thái tạm thời.
- **[DEFAULT-PROTOTYPE]** Khi chuyển đổi, actor giữ nguyên instance, chỉ rebind dữ liệu hiển thị và dữ liệu va chạm theo profile của nhân vật mới.
- **[FACT-AUDIT]** GDD v1 dùng lẫn lộn "nhân vật đang điều khiển" và "đội hình"; v1.1 tách hai khái niệm này (xem 3.2).

### 3.2 Chuyển nhân vật đang điều khiển khác với đổi đội hình

| Khái niệm | Định nghĩa | Khi nào được phép |
|---|---|---|
| **Active switch** | Đổi slot đang điều khiển trong bốn slot đã có | Trong khám phá và **trong suốt** encounter, kể cả boss |
| **Loadout change** | Thay đổi thành viên của bốn slot | Ngoài encounter; **[DEFAULT-PROTOTYPE]** bị khoá khi bước vào boss encounter |

Lý do tách: nếu khoá luôn active switch trong boss thì cơ chế lõi không thể được kiểm chứng ở đúng nơi nó quan trọng nhất. Nếu cho đổi roster giữa boss thì thiết kế boss và việc đo "lợi thế của switching" đều nhiễu.

### 3.3 Vị trí và va chạm khi chuyển đổi

- **[DEFAULT-PROTOTYPE]** Giữ nguyên vị trí actor nếu vị trí đó hợp lệ cho profile va chạm của nhân vật mới.
- Nếu profile va chạm mới không tương thích hình học hiện tại, thử một tập vị trí ứng viên có giới hạn: bán kính tìm kiếm tối đa **1.0 world unit**, thứ tự cố định (lên, sang phải, sang trái, các hướng chéo), chỉ chấp nhận ứng viên không overlap collider tĩnh và đứng trên bề mặt hợp lệ.
- Nếu không có ứng viên hợp lệ: **từ chối switch**, giữ nguyên nhân vật cũ, ghi lý do từ chối. Không được dịch chuyển không giới hạn, không được để overlap hình học, không được đặt vào khoảng không ngoài encounter bounds.
- **[OPEN]** Bán kính 1.0 và thứ tự ứng viên là giá trị khởi điểm, cần chỉnh theo kích thước nhân vật thực tế.
- **[OPEN]** Có nên hiển thị phản hồi (SFX/UI) khi switch bị từ chối hay im lặng — cần test khả năng đọc.

### 3.4 Bảng tham số chuyển đổi — mặc định prototype

Toàn bộ bảng này là **[DEFAULT-PROTOTYPE]**, không phải balance cuối.

| Tham số | Mặc định | Khoảng điều chỉnh | Ghi chú |
|---|---|---|---|
| Trạng thái được phép switch | idle, move, windup, recovery | thêm/bớt từng trạng thái | active frames và hitstun bị chặn |
| Switch trong active frames | bị chặn | bật/tắt | bật = cancel đòn đánh |
| Switch trong hitstun | bị chặn | bật/tắt | bật = thoát hitstun bằng switch |
| Switch khi nhân vật đã chết | bị chặn | — | chết = không thể là active |
| Switch lock sau khi switch | 0.15 s | 0.10–0.30 s | chống switch dồn trong một frame |
| Switch cooldown | 0.25 s | 0–1.0 s | 0 = không cooldown |
| HP nhân vật inactive | giữ nguyên, không hồi | thêm hồi 0–2%/s | switch không được hồi máu |
| Cooldown khi inactive | tiếp tục tick 100% | 0% / 50% / 100% | 0% = đóng băng cooldown |
| Trùng character ID trong 2 slot | bị từ chối | cho phép | prototype chọn invariant chặt |
| Đổi loadout khi vào boss | bị khoá | cho phép | active switch vẫn bật |
| Input ability trong lúc switch lock | bị bỏ qua, không buffer | buffer / bỏ qua | xem O-03 |

### 3.5 Điều còn mở

Xem bảng 13.1: O-01 (luật cancel cuối), O-02 (cooldown và balance cuối), O-03 (ability trong các trạng thái switch), O-04 (vai trò và balance bốn nhân vật).

## 4. Các hướng combat đang được đánh giá

### 4.1 Ba hướng

| Hướng | Mô tả | Điểm mạnh giả định | Rủi ro chính |
|---|---|---|---|
| **A — Platform Action RPG** | Nhảy, né, điều khiển theo chiều cao; địa hình ảnh hưởng chiến đấu | Hỗ trợ khám phá và traversal; không gian chiến đấu giàu hơn | Chi phí điều khiển và đọc đòn cao hơn; phụ thuộc độ chính xác nhảy, đặc biệt trên touch |
| **B — Combat Arena** | Vùng chiến đấu phẳng; tập trung tấn công, né, switch, đọc đòn; boss chia pha rõ | Dễ đọc, dễ kiểm soát chất lượng, rẻ hơn để làm đúng | Giảm vai trò của traversal trong chiến đấu |
| **C — Hybrid** | Khám phá dùng traversal platform; một số boss dùng arena thiết kế riêng, **dùng chung một lớp combat resolution** | Giữ được cả hai lợi thế nếu ranh giới code đúng | Dễ trượt thành hai codebase; camera và movement dễ phình |

### 4.2 Dùng chung và khác biệt

Chung cho cả ba: dữ liệu nhân vật, party state, ability definition, damage/hitbox/i-frame/cooldown, encounter state, telemetry.

Được phép khác: movement capability (có/không nhảy, ground check, slope, dodge dash), phạm vi camera, bố cục encounter, cách enemy di chuyển trong không gian.

Quy tắc bắt buộc: **combat resolution không được đọc trực tiếp Rigidbody hay trạng thái platform.** Mọi nhu cầu như "đang ở trên không" phải đi qua một provider/interface do lớp movement cung cấp. Nếu không, arena mode sẽ phải giả lập physics để chạy cùng một đoạn code combat.

### 4.3 Cổng quyết định

Chỉ chọn hướng sau khi có báo cáo so sánh theo mục 8, dựa trên dữ liệu chạy thật và self-play có ghi lại. Không chọn theo tổng điểm trung bình; các lỗi nghiêm trọng về điều khiển, khả năng đọc hoặc khả năng mở rộng phải được xử lý trước.

## 5. Phạm vi prototype

### 5.1 Trong phạm vi

- Một scene graybox duy nhất.
- Hai zone trong scene đó: **Zone T** (traversal) và **Zone A** (arena).
- Bốn nhân vật thử nghiệm với dữ liệu khác nhau tối thiểu, mỗi nhân vật một ability riêng đơn giản.
- Một loại enemy thường.
- Một elite/mini-boss có ít nhất một đòn báo trước và ít nhất một trạng thái dễ bị phản công.
- Chuyển đổi nhân vật, HP, sát thương, trạng thái chiến đấu, thắng/thua, reset encounter.
- Input theo hành động gameplay, dùng Input System hiện có.
- Telemetry cục bộ chỉ dành cho development (mục 9).
- Kiểm thử tự động cho các invariant ở mục 7.

### 5.2 Định nghĩa scene

**[DEFAULT-PROTOTYPE]** Một scene, hai zone, cùng một bộ dữ liệu party và cùng một lớp combat resolution. Khác biệt giữa hai zone chỉ nằm ở movement capability, camera strategy, bố cục hình học và cách enemy di chuyển.

Không dùng hai scene riêng, vì mục tiêu là chứng minh **một** lớp combat phục vụ được cả hai kiểu không gian. Nếu hai zone nằm ở hai scene, việc chuyển scene sẽ che mất lỗi chuyển mode.

### 5.3 Camera

**[DEFAULT-PROTOTYPE]** Camera dùng một host duy nhất, nhưng hành vi phải nằm sau một ranh giới strategy rõ ràng: exploration follow, arena framing, boss framing. Không nhúng các nhánh mode khác nhau vào một `LateUpdate` nguyên khối. Không giả định Cinemachine hay bất kỳ package nào chưa cài.

### 5.4 Loại trừ rõ ràng

Không làm trong prototype: inventory/equipment, gacha/banner/pity/IAP, world map, quest, economy, Supabase, cloud save, multiplayer, art production, pixel-perfect pipeline, bộ 20 nhân vật, localization, Addressables, save system.

### 5.5 Lập trường art

Prototype dùng graybox/programmer art. Hướng nghệ thuật mục tiêu vẫn giữ nguyên như 1.1 nhưng **không** chốt trong phase này: pixel-perfect rendering, PPU, độ phân giải tham chiếu, lighting asset, pipeline import sprite/animation đều bị hoãn sang một art/technical spike riêng sau khi hướng combat được chọn.

Lý do: các quyết định đó phụ thuộc vào hướng combat và vào ngân sách nội dung, và việc chốt sớm sẽ khoá cứng pipeline trước khi có bằng chứng.

## 6. Mô hình dữ liệu khái niệm

Đây là mô tả khái niệm, **không** phải yêu cầu tạo class, ScriptableObject hay asmdef ở phase này.

| Thành phần | Trách nhiệm | Nội dung tối thiểu |
|---|---|---|
| CharacterCatalog | Danh mục nhân vật, tra cứu theo ID ổn định | tập definition, kiểm tra ID trùng/thiếu |
| CharacterDefinition | Dữ liệu tĩnh của nhân vật | ID ổn định, stats nền, danh sách ability, tham số switch, tag vai trò thử nghiệm, tham chiếu hiển thị |
| CharacterRuntimeState | Trạng thái sống của một nhân vật | character ID, HP hiện tại/HP tối đa, trạng thái chiến đấu, cooldown từng ability, i-frame/stagger timer, buff tạm thời |
| PartyState | Tình trạng đội hình | 4 slot, character ID mỗi slot, active slot, cờ khoá loadout, lý do không hợp lệ |
| AbilityDefinition | Dữ liệu tĩnh của ability | ID, loại hành động, tham số sát thương/hiệu ứng, cooldown, điều kiện dùng, tham chiếu animation/FX |
| EncounterDefinition | Dữ liệu encounter | zone, spawn point, danh sách enemy, phase, điều kiện thắng/thua/reset |
| EncounterState | Trạng thái encounter đang chạy | phase hiện tại, enemy còn sống, kết quả, số lần reset |
| InputAdapter | Chuyển Input System thành sự kiện gameplay | ánh xạ action → ý định, không chứa luật gameplay |
| Presentation | Hiển thị | bind sprite/animation/UI theo state, không quyết định luật |
| CombatResolver | Phân giải sát thương/trạng thái | hitbox/hurtbox, thứ tự áp dụng hiệu ứng, hitstop, i-frame |
| TestInstrumentation | Ghi sự kiện phục vụ đo lường | xem mục 9 |

### 6.1 Invariant dữ liệu

- Character ID là duy nhất và ổn định; ID là khoá tham chiếu duy nhất, không dùng index mảng làm định danh.
- PartyState không bao giờ chứa ID không tồn tại trong catalog; slot không hợp lệ phải bị từ chối tại thời điểm gán, không phải phát hiện muộn trong combat.
- Runtime state không được chứa tham chiếu tới đối tượng Unity trong hình dạng sẽ được serialize (nếu sau này serialize).
- Một nhân vật chỉ có đúng một runtime state, bất kể nó đang active hay inactive.

### 6.2 Điều chưa quyết trong data model

**[OPEN]** Hình dạng save DTO, versioning, migration, và cách biểu diễn buff/effect lâu dài. Không thiết kế ở phase này vì save không nằm trong prototype và schema backend chưa được duyệt.

## 7. Kiểm thử chức năng (Gate A)

Toàn bộ mục này là **kế hoạch**. Không có test nào đã chạy.

### 7.1 Kiểm thử tự động bắt buộc

| ID | Nội dung | Loại |
|---|---|---|
| T-01 | Party có đúng bốn slot hợp lệ | EditMode |
| T-02 | Character ID trùng bị từ chối | EditMode |
| T-03 | Mỗi slot trong bốn slot có thể trở thành active đúng cách | PlayMode |
| T-04 | Sau khi switch, tồn tại đúng một actor điều khiển được | PlayMode |
| T-05 | Input không bị mất hoặc kẹt qua switch | PlayMode |
| T-06 | HP được giữ nguyên qua switch, không hồi máu | PlayMode |
| T-07 | Cooldown được giữ nguyên và tiếp tục tick khi inactive | PlayMode |
| T-08 | Slot không hợp lệ và definition null bị từ chối an toàn | EditMode + PlayMode |
| T-09 | Switch sát tường và sát mép pit không tạo va chạm không hợp lệ | PlayMode |
| T-10 | Tấn công, nhận sát thương, thắng, thua, reset encounter hoạt động | PlayMode |
| T-11 | Reset encounter ba lần liên tiếp cho cùng trạng thái đầu | PlayMode |

Bổ sung bắt buộc ở EditMode: catalog không có ID trùng, không có tham chiếu null, mọi ability ID trong CharacterDefinition phân giải được.

### 7.2 Trường hợp thủ công bắt buộc

Switch trong windup; trong active frames; trong recovery; trong hitstun; khi nhân vật đã chết; khi đang trên không; trong lúc boss chuyển pha; hai lần switch trong cùng một frame; khi đang pause/menu; switch bằng touch trên Android.

Mỗi trường hợp ghi lại: hành vi quan sát được, hành vi mong đợi theo bảng 3.4, và lý do từ chối nếu bị từ chối.

### 7.3 Tiêu chí ổn định đo được

- 0 exception không được xử lý trong toàn bộ lượt chạy test đã định nghĩa.
- 0 sự kiện mất input trong một lượt chơi tay liên tục 10 phút.
- 3 lần reset encounter liên tiếp cho cùng trạng thái đầu: cùng HP, cùng cooldown, cùng vị trí, cùng phase, cùng số enemy.

Đây là **tiêu chí đề xuất để phê duyệt**, không phải kết quả.

### 7.4 Không được coi là bằng chứng

"Compile thành công", "không có exception", "PlayMode xanh", "một tester nói thấy ổn", "không crash" — không cái nào chứng minh Gate B hoặc Gate C. Chúng chỉ chứng minh Gate A ở mức invariant máy kiểm được.

## 8. Giao thức đánh giá combat

### 8.1 Biến thể và điều kiện

Ba biến thể được đánh giá: A (platform), B (arena), C (hybrid dùng chung lớp combat). Trong prototype, A và B được hiện thực bằng hai zone; C là cách đánh giá hai zone như một hệ thống duy nhất.

Điều kiện tương đương ở mức có thể: cùng bộ nhân vật, cùng bộ ability, cùng loại enemy, cùng số lượng enemy, cùng mục tiêu encounter, cùng thời lượng mục tiêu.

Không thể tương đương tuyệt đối: hình học không gian, cảm giác di chuyển, và độ khó đọc đòn. Phải ghi rõ khác biệt này trong báo cáo thay vì che bằng điểm trung bình.

### 8.2 Chỉ số ghi lại

- Sát thương nhận vào.
- Số đòn có báo trước bị trúng.
- Số lần switch tự nguyện.
- Số lần switch bị từ chối kèm lý do.
- Số lần input thất bại (không thực thi hành động đã bấm).
- Thời gian hoàn thành encounter.
- Kết quả thắng/thua và số lần thử.
- Tự đánh giá 1–5 về khả năng đọc đòn và độ chính xác điều khiển (được đánh dấu rõ là chủ quan).

### 8.3 Trình tự thực hiện

1. Chạy toàn bộ test tự động của mục 7 trước. Không đánh giá cảm giác trên một build vi phạm invariant.
2. Self-play có ghi lại, lặp lại được: cùng encounter, cùng thứ tự, ghi lại số liệu.
3. Đánh giá so sánh A/B/C trên cùng bộ chỉ số.
4. External playtest khi có điều kiện, và **trước** mọi cam kết sản xuất lớn.

Không yêu cầu 12 tester ngoài để hoàn thành prototype. Prototype phải kết thúc được bằng self-play và test tự động.

### 8.4 Mục tiêu số — chỉ là giả thuyết

Các ngưỡng như "sát thương nhận ít hơn 30%" hoặc "trúng đòn báo trước ít hơn 20%" hoặc "ít nhất 3 lần switch tự nguyện" là **giả thuyết thăm dò**, dùng để phát hiện tín hiệu, không phải cổng pass/fail tự động. Không dùng chúng để tuyên bố biến thể này "tốt hơn" khi chưa có đủ mẫu và chưa kiểm soát điều kiện.

### 8.5 Ma trận đánh đổi

| Tiêu chí | A Platform | B Arena | C Hybrid |
|---|---|---|---|
| Điều khiển | Nhiều trạng thái hơn, dễ sai | Ít trạng thái, dễ chính xác | Phụ thuộc ranh giới movement |
| Khả năng đọc | Có thể bị che bởi địa hình | Rõ nhất | Rõ trong arena, khó hơn khi traversal |
| Nhịp chiến đấu | Phụ thuộc thiết kế màn | Dễ kiểm soát nhịp | Khác nhau giữa hai zone |
| Hỗ trợ khám phá | Cao | Thấp | Cao nếu tách đúng |
| PC + gamepad | Tốt | Tốt | Tốt |
| Android touch | Rủi ro cao nhất | Rủi ro thấp nhất | Trung bình |
| Chi phí triển khai | Cao nhất | Thấp nhất | Trung bình đến cao |

### 8.6 Giới hạn phải công bố

Mọi báo cáo phải nêu: số mẫu, điều kiện chạy, ai chấm, điểm nào là chủ quan, điểm nào là máy đo, và điều gì không thể suy ra từ dữ liệu hiện có. Với self-play một người, kết luận chỉ mang tính định hướng.

### 8.7 Quy tắc loại trừ

Một biến thể bị loại bỏ bất kể điểm tổng nếu: điều khiển hoặc khả năng dùng trên thiết bị đích bị đánh giá ≤2/5; hoặc tồn tại lỗi lặp lại được về vị trí không hợp lệ; hoặc tồn tại lỗi mất input lặp lại được; hoặc khả năng đọc đòn ≤2/5 (chặn thiết kế boss).

## 9. Kế hoạch telemetry

### 9.1 Mục đích và giới hạn

Telemetry ở đây là **công cụ đo cho development**, không phải analytics sản phẩm, không phải framework dữ liệu, không gửi lên backend.

### 9.2 Sự kiện tối thiểu

| Sự kiện | Trường chính |
|---|---|
| session_start | session id, build id, scene, platform, input scheme |
| encounter_start | encounter id, zone, phase đầu |
| switch_request | slot nguồn, slot đích, trạng thái combat hiện tại |
| switch_commit | slot đích, thời điểm, HP/cooldown của cả hai nhân vật |
| switch_reject | lý do (cooldown, hitstun, dead, invalid slot, invalid position, loadout locked) |
| ability_cast | ability id, nhân vật, cooldown còn lại |
| attack | nguồn, hitbox id, có trúng hay không |
| damage | nguồn, đích, lượng, HP trước/sau, có phải đòn báo trước |
| encounter_phase | phase cũ, phase mới |
| encounter_end | kết quả thắng/thua, thời lượng, số lần thử |
| encounter_reset | lý do, số lần reset trong session |
| input_failure | action mong đợi, trạng thái, thời điểm |

### 9.3 Kiểm soát

- Định dạng JSONL, một sự kiện mỗi dòng, ghi cục bộ dưới thư mục dữ liệu ứng dụng.
- Có công tắc bật/tắt rõ ràng, mặc định **bật trong development build**, **tắt trong release build** trừ khi được phê duyệt riêng.
- Không gửi dữ liệu qua mạng. Không chứa định danh tài khoản, email, hay bất kỳ dữ liệu cá nhân nào.
- Giới hạn dung lượng mỗi session và số file lưu giữ để không làm đầy thiết bị.
- Không được trở thành dependency của gameplay: tắt telemetry không được thay đổi hành vi game.

**[OPEN]** Việc đọc/ghi file có thể gây giật khung hình trên Android nếu ghi đồng bộ mỗi sự kiện; cách ghi (buffer, flush theo lô) cần được chọn khi triển khai. Chưa quyết ở phase này.

## 10. Baseline kỹ thuật và phụ thuộc

### 10.1 Baseline tại thời điểm audit

Các mục dưới đây là **[FACT-AUDIT]**, xác minh bằng đọc file trong repo (không phải chạy Unity, không phải build).

| Hạng mục | Giá trị |
|---|---|
| Unity | 6000.6.5f1 |
| Render pipeline | URP 17.6.0, có Renderer2D (2D Renderer) |
| Color space | Linear |
| Input | Input System 1.20.0; activeInputHandler = 1, tức chỉ New Input System, legacy Input bị tắt |
| Input asset hiện có | Assets/Settings/InputSystem_Actions.inputactions, map Player và map UI |
| Android | scriptingBackend = IL2CPP |
| Code gameplay | không có |
| Test assembly | không có |
| Prefab / sprite / animation | không có |
| Scene | một SampleScene với Main Camera và Global Light 2D; không có EventSystem |
| Package thiếu | Cinemachine, 2d.pixel-perfect, Addressables, Localization, Newtonsoft JSON, DOTween, netcode, Supabase SDK |

### 10.2 Ranh giới khái niệm đề xuất

Đây là **đề xuất**, không phải asmdef đã tạo.

| Ranh giới | Trách nhiệm | Được phụ thuộc |
|---|---|---|
| Data/Catalog | definition, catalog, invariant dữ liệu | không phụ thuộc lớp nào |
| Combat resolution | damage, hitbox, hiệu ứng, hitstop, i-frame | Data |
| Party | PartyState, active switch, loadout, kiểm tra hợp lệ | Data, Combat (read-only state) |
| Movement | movement capability theo zone | Data, không phụ thuộc Combat |
| Input adapter | ánh xạ action → ý định | Data (tên action) |
| Presentation | animation, UI, VFX, audio | Data, Combat (read-only), Party (read-only) |
| Camera | strategy theo zone/trạng thái | read-only event từ Encounter |
| Encounter | phase, spawn, win/lose/reset | Data, Combat |
| Instrumentation | ghi sự kiện | read-only từ mọi lớp |

Quy tắc: Combat không phụ thuộc Movement, Input, Presentation, Camera. Movement không phụ thuộc Combat. Presentation không được quyết định luật.

Số assembly cụ thể **chưa chốt**; sẽ là quyết định triển khai, không phải quyết định thiết kế.

### 10.3 Phụ thuộc đã biết

| ID | Phụ thuộc | Trạng thái |
|---|---|---|
| D-01 | Input asset **thiếu action cho ability và cho việc chọn slot 1–4**. Hiện có Move, Look, Attack, Interact, Crouch, Jump, Previous, Next, Sprint. | Chưa sửa. Việc bổ sung action là thay đổi asset dùng chung, cần phê duyệt riêng khi bắt đầu triển khai. |
| D-02 | Không có Cinemachine → camera phải tự viết | Đã ghi nhận, không thêm package |
| D-03 | Không có 2d.pixel-perfect → không thể chốt cảm giác pixel | Hoãn sang art/technical spike |
| D-04 | Không có Addressables → tham chiếu asset trực tiếp ở prototype | Đã ghi nhận |
| D-05 | Không có EventSystem trong scene → UI sẽ cần input module phù hợp Input System | Phát hiện khi triển khai UI |
| D-06 | Android IL2CPP: thời gian build và kiểm tra stripping | Rủi ro trung bình, cần đo khi có build đầu tiên |

### 10.4 Hoãn có chủ ý

Pixel-perfect, PPU, độ phân giải tham chiếu, pipeline import Aseprite, cấu trúc animation, lighting 2D, post-processing: hoãn sang art/technical spike sau khi chọn hướng combat.

## 11. Ranh giới save, Supabase và IAP

Không có gì trong mục này được triển khai ở phase prototype.

### 11.1 Nguyên tắc dài hạn

- Local save lưu trạng thái chơi có thể phục hồi offline.
- Cloud save theo tài khoản, có version.
- Các thao tác gacha có giá trị tài khoản phải được backend xác nhận.
- Quyền sở hữu IAP dựa trên giao dịch đã xác minh.
- Reset ngày và thời điểm banner dùng thời gian phía server.
- Tiền tệ, pity và mọi thao tác cấp tài nguyên cần quy tắc chống xử lý trùng.

### 11.2 Không tạo ở phase này

Không schema production, không bảng, không policy, không RPC, không migration, không project Supabase, không SDK, không product ID IAP.

### 11.3 Điểm cần phê duyệt trước khi mở khoá

- Nghiệp vụ gacha, progression, inventory được duyệt.
- Chính sách pity và tỷ lệ được duyệt.
- Chính sách đối soát phần thưởng offline được duyệt.
- Đánh giá pháp lý/age rating cho cơ chế gacha trên nền tảng phân phối mục tiêu.

## 12. Kế hoạch Multi-Orchestrator

### 12.1 Vai trò

| Vai trò | Trách nhiệm | Quyền ghi |
|---|---|---|
| Root | sở hữu tài liệu, hợp nhất mâu thuẫn, chốt tiêu chí, báo cáo | không |
| Worker A | phản biện thí nghiệm combat và tiêu chí nghiệm thu | không |
| Worker B | phản biện data model, phụ thuộc kỹ thuật, rủi ro phạm vi | không |
| Reviewer | kiểm tra nhất quán nội tại, thách thức khẳng định không có bằng chứng | không |
| QA | đánh giá khả năng kiểm chứng của tiêu chí | không |
| Developer | chỉ ghi file khi có nhiệm vụ được duyệt, phạm vi rõ | có, theo phạm vi |

### 12.2 Trạng thái thực thi

Multi-pane Herdr **không khả dụng** trong phiên này: shell bị từ chối quyền, nên không thể chạy bước kiểm tra môi trường bắt buộc trước mọi thao tác pane. Theo quy tắc an toàn, không thao tác pane. Các nhánh phân tích được chạy bằng subagent song song và được ghi nhận là **subagent, không phải multi-pane**.

Không có worker nào thay đổi Unity project. Phase này chỉ tạo một file tài liệu.

### 12.3 Phân công khi triển khai (chưa được kích hoạt)

Worker A sở hữu combat resolution, party, input adapter. Worker B sở hữu data/catalog, encounter, instrumentation. Không hai agent nào ghi cùng một file. Scene, prefab, ProjectSettings, manifest và input asset cần phê duyệt riêng. Developer chỉ nhận nhiệm vụ đã duyệt, có tiêu chí nghiệm thu.

## 13. Quyết định mở và rủi ro

### 13.1 Quyết định mở

| ID | Câu hỏi | Phân loại | Khi nào giải quyết |
|---|---|---|---|
| O-01 | Luật switch/cancel cuối cùng (cancel active frames? queue? chặn hitstun?) | [OPEN] | sau Gate B |
| O-02 | Cooldown switch cuối và balance | [OPEN] | sau Gate B |
| O-03 | Ability có được dùng trong mọi trạng thái switch không | [OPEN] | sau Gate A, trước Gate B |
| O-04 | Vai trò và balance cuối của bốn nhân vật prototype | [OPEN] | sau Gate B |
| O-05 | Camera và traversal production | [OPEN] | sau Gate C |
| O-06 | Tỷ lệ gacha, pity, economy, sản phẩm IAP | [OPEN] | sau vertical slice |
| O-07 | Schema database production | [OPEN] | sau khi nghiệp vụ được duyệt |
| O-08 | Chính sách đối soát phần thưởng offline | [OPEN] | trước cloud save |
| O-09 | Có thêm package (Cinemachine, pixel-perfect, Addressables) hay tự viết | [OPEN] | sau Gate C, cần phê duyệt |
| O-10 | Mức Android hỗ trợ trong prototype (touch tối giản ở mức nào) | [OPEN] | trước khi build Android đầu tiên |

### 13.2 Rủi ro

| ID | Rủi ro | Mức | Giảm thiểu |
|---|---|---|---|
| R-01 | Luật switch chưa chốt khiến Gate A không có tiêu chí nhị phân | Cao | bảng 3.4 làm mặc định tạm; test được tham số hóa |
| R-02 | Đo lường Gate B bằng self-play dễ thành giai thoại | Cao | telemetry + công bố n và giới hạn |
| R-03 | Hybrid trượt thành hai codebase | Cao | ranh giới movement/combat + quy tắc không đọc physics từ combat |
| R-04 | Camera tự viết phình to | Trung bình | strategy boundary, không nhánh mode trong một LateUpdate |
| R-05 | Android touch không đủ dùng | Trung bình | test khả thi tối giản sớm, không làm UI đẹp |
| R-06 | Roster 20 nhân vật không có đòn bẩy cơ chế ngoài sưu tầm | Trung bình | không chặn prototype; phải giải trước cam kết gacha |
| R-07 | Chi phí art/animation cho roster vượt năng lực sản xuất | Cao | chưa cam kết số nhân vật có art thật |
| R-08 | Không có thiết bị Android mục tiêu và ngân sách khung hình | Trung bình | chốt device profile trước khi chấm tiêu chí thiết bị |
| R-09 | Charging/thermal khi ghi telemetry trên mobile | Thấp | ghi theo lô, giới hạn dung lượng |

## 14. Go/No-Go để cấp phép triển khai

Triển khai prototype chỉ được cấp phép khi tất cả điều kiện sau đúng:

1. Tài liệu này được phê duyệt, bao gồm bảng mặc định 3.4 và mục 7.
2. Ba hướng combat và bộ tiêu chí so sánh ở mục 8 được chấp nhận.
3. Bảng tham số 3.4 được chấp nhận là **mặc định thử nghiệm**, không phải luật cuối.
4. Ranh giới khái niệm ở 10.2 được chấp nhận; số assembly cụ thể để lại cho bước triển khai.
5. Danh sách test 7.1 và tiêu chí ổn định 7.3 được chấp nhận.
6. Telemetry mục 9 được chấp nhận là phạm vi bổ sung.
7. Việc bổ sung action còn thiếu trong input asset (D-01) được phê duyệt riêng.

Khi được cấp phép, phạm vi triển khai bị giới hạn ở mục 5. Mọi thứ ngoài mục 5 vẫn bị loại trừ, kể cả khi có vẻ cần thiết cho "đầy đủ".

## Appendix A — Thay đổi so với v1

| Mã | Thay đổi |
|---|---|
| E1 | Tách "luật cuối" khỏi "giao thức thí nghiệm". §3 v1 giữ nguyên tính mở; §9 v1 được thay bằng bảng mặc định 3.4 + giao thức đo ở mục 8. |
| E2 | Định nghĩa tường minh actor bền duy nhất, inactive là dữ liệu, HP/cooldown persist, cooldown tick khi inactive, tách active switch khỏi loadout change, khoá loadout ở boss, quy tắc giữ vị trí và giải toả có giới hạn. |
| E3 | Thêm kế hoạch telemetry cục bộ chỉ dev và phụ thuộc input còn thiếu; thêm yêu cầu test khả thi touch tối giản. |
| E4 | Chốt một scene hai zone; thêm ranh giới camera strategy; bỏ giả định Cinemachine. |
| E5 | Thay "hoạt động ổn định" bằng danh sách test tự động và ba tiêu chí ổn định đo được. |
| E6 | Bỏ ngưỡng pass/fail cứng và yêu cầu 12 tester; thêm quy trình tự động → self-play → external, kèm yêu cầu công bố giới hạn. |
| E7 | Tách hướng nghệ thuật khỏi kiểm chứng combat; hoãn pixel-perfect/PPU/lighting/pipeline. |
| — | Thêm mục phân loại quyết định, baseline kỹ thuật, ranh giới Supabase rõ hơn, decision/risk register, và điều kiện go/no-go. |

## Appendix B — Nguồn gốc và kiểm soát tài liệu

GDD v1 được cung cấp dưới dạng draft trong phiên thảo luận ngày 2026-10-21 và chưa từng được lưu trong repository. Tài liệu này là bản đầy đủ thay thế, không phải bản vá, và giữ lại mọi cam kết phạm vi của v1.

Các quan sát baseline trong mục 10.1 được xác minh bằng đọc file trong repository, không phải bằng chạy Unity Editor, compile, hay build. Không có test nào trong tài liệu này đã được thực thi.

Tài liệu nằm ngoài thư mục Assets để Unity không import và không sinh .meta.
