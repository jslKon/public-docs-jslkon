---
title: Clean Code
description: Clean code là gì, nguyên nhân code smell, SOLID, và practice về naming, function, abstraction, luồng điều khiển, error handling, code review.
tags:
  - clean-code
  - solid
  - code-review
date: 2026-09-13
---

> Ghi chép từ buổi **Clean Code** trong khoá **Ronin Engineer S02**. Buổi lý thuyết, không có code
> chạy. Tôn trọng tác giả: nội dung thuộc về giảng viên, đây chỉ là bản ghi chép lại để ôn.

## 1. Clean code là gì

> **Clean code = code dễ hiểu và dễ thay đổi.**

Bốn mục đích luôn nghĩ trong đầu khi gõ code, và đó là **đích**, mỗi người có con đường khác nhau
để đến:

| | |
|---|---|
| **Readable** | Dễ đọc |
| **Maintainable** | Dễ maintain |
| **Testable** | Dễ test |
| **Extensible** | Dễ mở rộng |

Mục đích của clean code giống clean architecture: **giảm công sức build và maintain hệ thống**.

### Tại sao cần

Hai vấn đề nhức nhối của hầu hết team dev là **nhiều bug** và **chậm đưa sản phẩm ra thị trường**.
Một nguyên nhân là code không clean: người maintain rất khó khăn và mất thời gian.

> Nếu bạn nghĩ kiến trúc tốt là tốn kém, hãy thử một kiến trúc tồi. — Brian Foote

Xét **chi phí trên mỗi thay đổi theo thời gian**: dirty code ban đầu tốn ít công, nhưng về sau
maintain hay thêm feature rất tốn. Clean code cũng tăng theo thời gian nhưng **tăng chậm**.

### Đo clean thế nào

Khái niệm trừu tượng và **mang tính quan điểm**: mình thấy clean, người khác thấy không, không ai
sai. Nhưng nhiều người cùng đồng ý clean thì là clean. Đơn vị đo vui: **số câu "WTF" mỗi phút**
khi đọc hoặc review code.

Điều kiện tiên quyết là có **sense of beauty**, cảm nhận về cái đẹp. Cảm nhận này được **kinh
nghiệm** hình thành dần.

### Hai trade-off người mới hay băn khoăn

| | Kết luận |
|---|---|
| Clean code vs **performance** | Tách function, method, class **gần như không ảnh hưởng**. Về lý thuyết assembly có thêm lệnh jump nhưng thực tế không đáng kể. Chỉ khi **đổi logic** cho rõ ràng mà logic mới chậm hơn thì mới ảnh hưởng |
| Clean code vs **time-to-market** | Code nhanh không clean thì xong task sớm nhưng gây lỗi, quay lại fix bug **còn mất thời gian hơn**. Khi gấp vẫn phải giữ tối thiểu: **không bug, dễ đọc, dễ test**. Có thể tạm bỏ khả năng mở rộng, sau quay lại sửa còn đơn giản |

## 2. Nguyên nhân code smell

| Nhóm | Nguyên nhân | Chi tiết |
|---|---|---|
| **Bị động** | **Thiếu cảm nhận về cái đẹp** | Do mindset và hiểu biết chưa đủ. Cách rèn: luyện tập; luôn hỏi **tại sao, để làm gì** khi đọc code người khác; đọc sách |
| **Chủ động** | **Độ phức tạp cao** | (1) **Dependency**: dùng nhiều thư viện ngoài mà chỉ biết input output, không biết bên trong, nên logic bị che và đổi version thì ảnh hưởng mọi chỗ dùng. (2) **Mập mờ, không thống nhất**: cùng một function trên đặt `message`, dưới đặt `msg`, người đọc từ giữa tưởng chỉ có một biến |

Khó khăn lớn nhất khi làm phần mềm là **handle độ phức tạp**. Viết clean code đồng nghĩa với có
kỹ thuật **giảm độ phức tạp**.

## 3. Làm sao viết được clean code

Theo đúng định nghĩa, kỹ thuật chia hai nhóm:

| Để **dễ hiểu** | Để **dễ thay đổi** |
|---|---|
| Naming, convention, format, code review, static scan | Principles, design pattern, test, refactor |

Cách rèn thực tế:

- **Đọc thật nhiều.** Cả code clean lẫn code smell để phân biệt và **định hình quan điểm** của
  mình. Đọc code **open source** của main contributor. Đọc code bạn bè, **pair programming**.
- **Đọc sách**, cả tech và non-tech.
- **Kỷ luật nhưng thực dụng.** Clean code là một **chặng đường**. Kỷ luật là mỗi khi viết thì nghĩ
  đến principle và convention; convention thành **kỹ năng** rồi thì không phải nhớ nữa. Đừng cố
  clean tuyệt đối ngay, phải **cân bằng với tốc độ hoàn thành task**.
- Cần cả **nền tảng kỹ thuật và kiến thức domain**. Code là giải bài toán business, hiểu business
  mới dùng đúng từ chuyên môn để đặt tên.
- **Refactor thường xuyên**, cũng là lúc nhìn lại tư duy mình thay đổi thế nào.

## 4. Principles

### KISS

1. **Chia vấn đề to thành vấn đề nhỏ** dễ hiểu, dễ giải.
2. **Không over-engineering.** Bài toán A có thể phát sinh B, C trong tương lai với xác suất
   khoảng 10% thì **chỉ giải A**. Nghĩ giải pháp phức tạp rất mất thời gian, mà một task còn phải
   chia thời gian cho nghĩ solution, implement, kiểm thử, mô tả cho mọi người.

### SOLID

Bộ principle **ngôn ngữ nào cũng nên học**, không riêng Java.

| | Ý chính |
|---|---|
| **S** — Single Responsibility | Một lớp **một trách nhiệm, một lý do để thay đổi**. Validation chỉ validation, mapper chỉ mapper |
| **O** — Open/Closed | Khuyến khích **mở rộng** (class mới, method mới), không khuyến khích **sửa method đã chạy ổn**. Muốn đổi logic method cũ thì thêm class mở rộng với method mới, logic cũ vẫn chạy. Người mới hay mắc: có yêu cầu mới là sửa cái cũ |
| **L** — Liskov Substitution | Object lớp con **thay thế được** lớp cha mà không ảnh hưởng tính đúng đắn. Lớp con override phải **giữ logic** của cha rồi thêm logic riêng, không override rỗng hay throw. Mục đích là **giảm độ phức tạp**: gọi method ở cha hay con đều cùng logic, hệ thống không phát sinh logic mới |
| **I** — Interface Segregation | Tách interface to thành **interface nhỏ có mục đích cụ thể**. Interface to **rất khó đặt tên** vì method ở nhiều hoàn cảnh khác nhau. Lưu ý: "tái sử dụng" **không phải** lý do, vì method vốn đã chỉ tồn tại ở một chỗ |
| **D** — Dependency Inversion | (1) Phụ thuộc **interface hoặc abstract**, không phụ thuộc implementation: code bên trong gọi interface, không import implementation trực tiếp. (2) **Chiều phụ thuộc từ ngoài vào trong**: module ngoài phụ thuộc module trong, trong không import ngoài. "Cao thấp" tuỳ góc nhìn, quan trọng là có **một chiều phụ thuộc** |

Hầu hết principle đều nhằm **giảm độ phức tạp**, mỗi principle ở một khía cạnh. Chi tiết về chiều
phụ thuộc xem trang [Codebase Structure](/documents/code-quality/codebase-structure).

## 5. Naming

> Trong computer science có hai thứ khó: cache invalidation và naming things.

| Tip | Ví dụ |
|---|---|
| **Tên có ý nghĩa theo ngữ cảnh** | `boolean scope = checkScope()` nên là **`isScope`**: biến boolean, tiền tố `is` phản ánh đúng sai |
| **Tránh tên ít hàm lượng thông tin** | `data` thì biến nào cũng là dữ liệu. Dùng `payload` để người đọc hiểu ngay đây là body của message |
| **Hạn chế viết tắt** | Java prefer viết đầy đủ `message`, JavaScript quen `msg`. Viết tắt bắt não thêm một bước suy ra từ gốc. Từ quá quen thì ổn, nhưng `v`, `d` thì không nhớ hết |
| **Không lặp prefix của class trong tên method** | `AuthController.authLogin` bỏ prefix thành `login`. Tên bắt đầu bằng chữ khác nhau thì lướt mắt phân biệt được ngay |
| **Thống nhất** | Không `message` ở trên rồi `msg` ở dưới trong cùng một method |

## 6. Function và class

### Function

- **Viết như kể chuyện**: có đầu có cuối, đọc từ trên xuống.
- **Một function một nhiệm vụ** → dễ đặt tên.
- Nếu được thì viết **pure function**, như biểu thức toán vào x ra y.
- **Tách command và query.** Ví dụ thật: method `getUser` vừa đọc DB vừa cập nhật số lần login.
  Hàm tên `get` thì chỉ get, thêm logic ghi vào là tên không còn phản ánh đúng ý nghĩa.
- **Function nhỏ** thì dễ test và dễ đặt tên, context bé thì tìm từ dễ. Quan điểm giảng viên:
  **≤ 40 dòng logic**, không tính dòng trống và dòng ngoặc.
- **Một switch hoặc một cụm if-else** mỗi function.
- **Tối đa 3 tham số.**
- **Tránh điều kiện lồng**, refactor để phá lồng.
- **Hạn chế callback** phức tạp trong một function.
- Khai báo function để **không lặp code**.
- Khi cần sửa thì theo **Open/Closed**: tạo class hoặc method mới để A dùng cũ, B dùng mới, cùng
  tồn tại.

### Class

Class cũng nhỏ, mỗi class một nhiệm vụ. Dấu hiệu class đang ôm nhiều hơn một nhiệm vụ là **khó
đặt tên** cho nó, lúc đó cân nhắc tách.

Thứ tự các thành phần từ trên xuống:

```
static final
attribute public
attribute private
constructor
method public
method private
```

Public phơi ra ngoài nên để trên, private che đi nên để dưới.

## 7. Abstraction

Một function dài làm pizza: check order loại rau hay thịt để chọn topping, nhào bột, tạo lò, làm
nóng lò, cho vào lò, đợi, lấy ra, set trạng thái đã nướng, đóng hộp. Nhìn qua đã dài, đọc rất mệt.

Refactor bằng cách **khái quát các bước tổng quát** thành function: `preparePizza` (nhào bột,
topping), `bakePizza` (tạo lò, làm nóng, cho vào, đợi, lấy ra), `boxPizza`. Đọc hàm tổng thấy ngay
ba bước; lỗi ở bước nào thì vào đúng function đó.

**Abstraction là con dao hai lưỡi.** Khái quát tốt thì dễ hiểu; khái quát chưa tốt thì càng phức
tạp. Gộp nướng cộng đóng hộp thành `makePizza` là tên mơ hồ, phải đọc bên trong mới hiểu. Với ba
bước rõ: qua bước 1 có pizza (chay hay thịt không quan trọng), bước 2 input pizza output pizza đã
nướng, bước 3 đóng gói.

Abstraction **giảm độ phức tạp**, và làm phần mềm là khái quát rất nhiều. **System design cũng là
abstraction**: biết service này nhiệm vụ gì mà không cần biết hết logic bên trong. Phải **nắm chắc
logic bên trong** mới abstract hiệu quả, nên nó phụ thuộc thông tin có được và kinh nghiệm.

## 8. Đơn giản hoá luồng điều khiển

**Ví dụ 1, tạo booking.** Check user active, check ghế đã book chưa, qua hết thì tạo booking. Viết
if-else lồng thì rối. Dùng **early return**: mỗi điều kiện fail thì `return` hoặc `throw` dừng
ngay, **không dùng else**, **happy path để ngoài cùng**.

**Ví dụ 2, vòng while.** Trong while có if-else "ghế có người ngồi thì làm nóng, không thì làm
mát", rồi if in phiếu sức khoẻ, rồi if break. Vẽ luồng ra thấy rất nhiều nhánh. Giải: tách if-else
nhiệt độ thành function **`adjustSeatTemperature`**. Logic **không mất đi, chỉ chuyển chỗ**, nhưng
function while còn rất đơn giản. Đây cũng là một bước abstraction.

## 9. Error handling

- **Đặt try-catch lên đầu** function, ngay sau các khai báo. Đọc đầu function là biết ngay có
  ngoại lệ gì cần nắm.
- Logic trong `try` như một **transaction**: tất cả thành công thì thành công, một cái fail thì
  **rollback** trạng thái cũ.
- Bắt exception thì show lỗi **rõ ràng**: loại gì, ở đâu, tại sao.
- **Xử lý tập trung**: lỗi nào xử lý chung được thì văng ra cho một **handler tập trung**, không
  catch từng chỗ.
- Hai loại checked và unchecked exception, tự tìm hiểu thêm.

## 10. Comment, format, boundary, refactor

**Comment** cho thuật toán và mô tả interface (đầu vào, đầu ra, logic tổng quan). **Không comment
chi tiết bên trong method**, để code tự giải thích.

**Format** tuỳ ngôn ngữ và convention team, 2 hay 4 space không có đúng sai. Giải thích vui: ngày
xưa màn hình bé, 2 space nhìn được nhiều code hơn; IntelliJ hiện đại màn hình rộng nên 4 space
tách bạch layer rõ hơn. Quan trọng là **cả team một format**, hai người hai format sửa cùng file
là **conflict** ngay, resolve tốn công vô ích. Dùng **linter** tránh lỗi lặt vặt.

**Test giúp code dễ đọc**: chưa hiểu code nhưng nhìn test biết đầu vào đầu ra là đoán được ý nghĩa
method.

**Boundary** là ranh giới giữa code mình và code thư viện, tức chỗ import class của thư viện. Core
cần **ổn định nhất**; thư viện thay đổi mà dùng trực tiếp khắp nơi thì **ảnh hưởng toàn app**.
Giải: **adapter** wrap class, interface và exception của thư viện thành của mình.

**Refactor** từng tí một, không refactor một cục. Bước: nắm luồng xử lý và **document lại** → sửa
→ **test**. Test bằng tay không đủ, **phải có unit test** để chạy lại cái cũ tự động. Mỗi lần sửa
**tối đa 10 file, độ sâu class 3**.

## 11. Code review

### Mục đích

- Đảm bảo **chất lượng code base**, giữ **principle, convention, kiến trúc** không bị vi phạm.
- Giúp bản thân phát triển nhanh hơn: học kỹ năng cứng và luyện **kỹ năng mềm**, trình bày cho
  người khác hiểu và lắng nghe ý kiến.

### Khía cạnh đánh giá

1. Code **giải quyết được vấn đề**, đáp ứng chức năng, chạy được.
2. Có **ảnh hưởng thiết kế hoặc kiến trúc chung** không.
3. **Dễ đọc.**
4. **Tính thống nhất**, follow convention của team.
5. **Đã test chưa, đã document chưa.** Làm API thì phải có API doc.
6. Không chỉ mò lỗi, còn **ghi nhận kỹ thuật hay** và khuyến khích để mọi người học.

### Hai loại và quy trình

| Loại | Khi nào |
|---|---|
| **Peer-to-peer** | Một member được senior hoặc team lead review. Cho **thay đổi nhỏ** |
| **Group review** | Cả một **luồng user story 5 đến 10 API**, xong hết rồi cả nhóm ngồi review chung. Cồng kềnh, chậm hơn |

1. Dev hoàn thành tính năng, tạo **merge request** có thông tin cụ thể: task làm gì, input output.
2. Dev đảm bảo code **chạy được**, đã test các trường hợp, và **đã test trên môi trường dev**,
   không chỉ self-test local. Người mới hay bỏ bước này, rất nguy hiểm: code không chạy thì buổi
   review kết thúc rất nhanh, quay lại sửa rồi review lại, mất thời gian mọi người.
3. Thông báo cho reviewer. Reviewer có **checklist** theo tính năng.
4. Reviewer đưa ra điểm cần sửa. **Chỉ sửa khi cả hai bên đồng ý.** Chưa thoải mái thì trao đổi
   thêm cho rõ **tại sao**, hiểu tại sao mới fix hiệu quả và mới học được.
5. Sửa xong thì **approve**.

### Thái độ và cách comment

- **Lịch sự**, tinh thần đóng góp và cùng giải quyết vấn đề, không phải đi tìm khuyết điểm.
- Comment **tránh câu mệnh lệnh**, dùng câu **gợi ý**: "Theo mình nên làm thế này thì tốt hơn,
  vì…" thay cho "Em phải làm thế này".
- Comment kèm **tại sao** và **hướng tiếp cận**, không nhất thiết phải là giải pháp đầy đủ.
- Phân loại: **TODO** cho việc bắt buộc, **nit** hoặc **optional** cho việc nhỏ, **FYI** cho việc
  sửa hay không cũng được. Lý thuyết là vậy nhưng giảng viên khuyên **cứ đánh TODO hết**, đánh
  loại khác thì nhiều khi không có tâm lý sửa.

### Practice

- **Thay đổi càng nhỏ càng tốt**, diff dài thì ai đọc cũng nản.
- **Có checklist** cho tính năng to, và luyện tập để review nhanh.
- **Xung đột ý kiến**: trước hết **lắng nghe để hiểu ý tưởng của cả hai bên** rồi mới đánh giá.
  Không chỉ so giải pháp của mình với của bên kia mà **nghĩ thêm giải pháp khác**, đánh giá ưu
  nhược từng cái, rồi **sắp xếp độ ưu tiên các yếu tố theo ngữ cảnh**. Ưu tiên có **tính thời
  điểm**: nay thứ tự này, sau có thể khác.
- **Vẫn không có tiếng nói chung**: làm **POC** nếu rẻ, vì bàn luận chỉ dựa trên giả định còn thực
  tế mới thuyết phục; hoặc **phơi vấn đề ra team, tech lead, SA, manager** để có góc nhìn khách
  quan hơn.

## 12. Chiến lược code

Cách cân bằng clean code với tốc độ hoàn thành task:

```
code cho chạy → refactor một chút → test → submit task (tester vào)
→ quay lại refactor thêm → test lại
```

Không mất quá nhiều thời gian refactor **trước khi** submit. Code xong chưa chạy vội, nhìn lại một
chút: chỗ đặt tên chưa ổn thì sửa, code dính thì tách block.

Refactor sau submit **phải test lại**. Sửa mà không test, tester đã test xong lại hỏng, là **sai
quy trình và ảnh hưởng người khác**.

## 13. Tổng kết

1. Clean code là **dễ hiểu, dễ thay đổi**. Bốn tiêu chí dễ đọc, dễ maintain, dễ test, dễ mở rộng
   là **đích**.
2. Cân bằng clean code với tốc độ bằng **chiến lược code** ở mục 12.
3. **Thống nhất** cách đặt tên, convention, cả tư duy, để giảm độ phức tạp.
4. Hai kỹ thuật áp dụng thường xuyên: **abstraction** và **đơn giản hoá luồng điều khiển**. Phải
   thực hành mới nhuyễn.

## Thử làm

Refactor một đoạn code trong dự án của bạn. Không có yêu cầu cụ thể, có thể nhiều class nhiều
function. Chọn **logic phức tạp**, đừng refactor getter và setter.

## Ghi chú bên lề

Khi chọn message broker, đánh giá theo thứ tự: **delivery guarantee** (at least once, at most
once, exactly once) → **xử lý lỗi** khi gửi thất bại → **performance**. Hầu hết broker chỉ đáp ứng
at least once hoặc at most once; Kafka đạt được cả hai. **Không broker nào đảm bảo exactly once**
ở tầng messaging.
