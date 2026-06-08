# Wealth Butler

Một trợ lý AI quản lý tài sản cá nhân theo triết lý:

> Không chỉ ghi nhớ các con số, mà còn lưu giữ câu chuyện đằng sau các quyết định tài chính.

## Mục tiêu

Wealth Butler được thiết kế để trở thành:

* Người lưu giữ ký ức tài chính cá nhân.
* Người theo dõi tài sản và nghĩa vụ tài chính.
* Người kết nối các quyết định trong quá khứ với mục tiêu trong tương lai.
* Hệ thống quản lý tri thức tài chính dựa trên Markdown và AI.

Dự án ưu tiên:

* Dữ liệu thuộc sở hữu hoàn toàn của người dùng.
* Lưu trữ bằng Markdown dễ đọc.
* Kiến trúc Event Sourcing.
* Có thể hoạt động với Local LLM hoặc Cloud LLM.

---

# Kiến trúc

Hệ thống được chia thành 3 lớp:

## 1. BUTLER.md (Constitution)

Định nghĩa:

* Sứ mệnh
* Giá trị cốt lõi
* Triết lý kiến thức
* Triết lý bộ nhớ
* Triết lý hội thoại

Đây là tài liệu có độ ưu tiên cao nhất.

---

## 2. DATA_MODEL.md (Ontology)

Định nghĩa:

* Event
* Asset
* Liability
* Receivable
* Goal
* Journal
* Decision

Đây là lớp mô hình dữ liệu của hệ thống.

---

## 3. OPERATING_RULES.md (Behavior Layer)

Định nghĩa:

* Quy trình hội thoại
* Cách ghi nhận sự kiện
* Cách xử lý dữ liệu thiếu
* Quy trình Correction Event
* Quy tắc cập nhật Portfolio
* Quy tắc theo dõi Goals

---

# Cấu trúc thư mục đề xuất

```text
/
├── README.md
├── BUTLER.md
├── DATA_MODEL.md
├── OPERATING_RULES.md
│
├── events/
├── assets/
├── liabilities/
├── receivables/
├── goals/
├── journals/
├── decisions/
│
└── snapshots/
    ├── portfolio.md
    ├── active_goals.md
    └── outstanding_receivables.md
```

---

# Sử dụng với Cline

Mở thư mục dự án trong VSCode.

Đảm bảo các file sau tồn tại:

* BUTLER.md
* DATA_MODEL.md
* OPERATING_RULES.md

Sau đó mở một phiên chat mới trong Cline và sử dụng prompt sau.

# Prompt khởi động

```text
Act as the Wealth Butler.

I will be the user.
Only behave as the actual Butler.

Load and follow these documents in order of precedence:

1. BUTLER.md (Constitution)
2. DATA_MODEL.md (Ontology / Source of Truth)
3. OPERATING_RULES.md (Behavior Rules)

When conflicts occur:
BUTLER.md > OPERATING_RULES.md > DATA_MODEL.md

Additional runtime constraints:

* Communicate in Vietnamese.
* Response must be under 100 words unless the user explicitly asks for analysis.
* Capture first, refine later.
* Never block event recording.
* Show exact markdown files created or modified.
* Ask at most one follow-up question.
* Missing information may remain Unknown or Pending Clarification.
* Prioritize speed over completeness.
* Apply CorrectionEvent rules when facts are corrected.
* Treat Portfolio, Goals, and Receivables as derived views.
* Do not explain internal architecture unless asked.

Begin acting as the Butler immediately.
```

---

# Ví dụ hội thoại

User:

> Tôi vừa mua 2 chỉ vàng.

Butler:

> ✅ Đã ghi nhận.
>
> Updated:
>
> * events/EVENT_20260605_gold_purchase.md
> * assets/gold_sjc.md
> * snapshots/portfolio.md

---

User:

> Tôi cho anh Nam mượn 25 triệu.

Butler:

> ✅ Đã ghi nhận.
>
> Updated:
>
> * events/EVENT_20260605_loan_to_nam.md
> * receivables/REC_NAM.md
> * snapshots/outstanding_receivables.md

---

# Trạng thái hiện tại

Dự án đang ở giai đoạn MVP Validation.

Mục tiêu hiện tại:

* Kiểm chứng trải nghiệm thực tế.
* Thu thập lỗi và edge cases.
* Hoàn thiện mô hình dữ liệu.
* Tối ưu hành vi của Butler.

---

# Đóng góp

Mọi phản hồi đều được hoan nghênh.

Đặc biệt quan tâm đến:

* Các tình huống Butler hiểu sai ý người dùng.
* Các trường hợp dữ liệu bị lệch.
* Các luồng hội thoại gây khó chịu.
* Các vấn đề về mô hình dữ liệu.
* Các vấn đề về tính nhất quán giữa Event và Snapshot.

Hãy tạo Issue hoặc Pull Request để thảo luận.
