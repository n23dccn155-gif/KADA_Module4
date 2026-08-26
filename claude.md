# Module 4 — Day 1: Data Cleaning with AI (Claude)

## Thông tin

| Mục | Chi tiết |
|-----|---------|
| **Sinh viên** | N23DCCN155 — Phạm An |
| **Dataset** | Netflix Movies and TV Shows (`netflix_titles.csv`) |
| **AI Tool** | Claude (Anthropic) — sử dụng qua Antigravity IDE |
| **Ngày thực hiện** | 26/08/2026 |

---

## 1. Mô tả Dataset

Dataset `netflix_titles.csv` chứa thông tin về các bộ phim và chương trình TV trên nền tảng Netflix.

- **Số dòng (rows):** 8807
- **Số cột (columns):** 12
- **Các cột:** `show_id`, `type`, `title`, `director`, `cast`, `country`, `date_added`, `release_year`, `rating`, `duration`, `listed_in`, `description`

---

## 2. Các lỗi dữ liệu đã phát hiện (Step 2)

| Lỗi | Chi tiết |
|-----|---------|
| Dòng trùng lặp hoàn toàn | Kiểm tra bằng `df.duplicated().sum()` |
| `show_id` trùng lặp | Kiểm tra bằng `df['show_id'].duplicated().sum()` |
| Dữ liệu thiếu (missing) | Nhiều cột bị thiếu: `director`, `cast`, `country`, `date_added` |
| Sai kiểu dữ liệu | Cột `date_added` đang là kiểu `object` (chuỗi) thay vì `datetime` |

---

## 3. Các quy tắc làm sạch đã áp dụng (Step 3)

### Rule 1: Chuyển đổi kiểu dữ liệu cột `date_added`
```python
df['date_added'] = pd.to_datetime(df['date_added'].str.strip(), errors='coerce')
```
- **Lý do:** Cột `date_added` đang lưu dưới dạng chuỗi (object), cần chuyển sang `datetime64` để hỗ trợ phân tích theo thời gian (lọc, sắp xếp, trích xuất năm/tháng...).
- **Xử lý lỗi:** Tham số `errors='coerce'` chuyển các giá trị không parse được thành `NaT` thay vì gây crash.

### Rule 2: Điền giá trị thiếu cho các cột văn bản
```python
cot_can_dien = ['director', 'cast', 'country']
df[cot_can_dien] = df[cot_can_dien].fillna('Unknown')
```
- **Lý do:** Các cột `director`, `cast`, `country` có nhiều giá trị bị thiếu (NaN). Điền `'Unknown'` giúp giữ nguyên số lượng dòng, tránh mất dữ liệu khi phân tích, và giúp nhận diện rõ ràng dữ liệu nào chưa có thông tin.

---

## 4. File đầu ra

| File | Mô tả |
|------|-------|
| `netflix_titles.csv` | File dữ liệu gốc (KHÔNG bị thay đổi) |
| `netflix_analysis.ipynb` | Notebook chứa toàn bộ 4 bước phân tích và làm sạch |
| `netflix_titles_clean.csv` | File dữ liệu đã được làm sạch |
| `claude.md` | File tài liệu mô tả quy trình (file này) |

---

## 5. Vai trò của AI trong quy trình

Claude đã hỗ trợ trong các bước sau:
1. **Đọc và mô tả tổng quan dữ liệu** — sinh code đọc CSV, in shape và dtypes.
2. **Phát hiện lỗi dữ liệu** — sinh code kiểm tra duplicate, missing values, sai kiểu dữ liệu.
3. **Đề xuất và áp dụng cách sửa lỗi** — đề xuất 2 rule phù hợp với dataset Netflix và sinh code thực thi.
4. **Lưu kết quả** — sinh code export file CSV sạch ra ổ đĩa.

> **Ghi chú:** Toàn bộ quy trình được thực hiện theo phương pháp "Prompt → AI sinh code → Chạy và kiểm tra → Lặp lại" — giúp sinh viên hiểu cách tận dụng AI như một công cụ hỗ trợ phân tích dữ liệu.
