# 🛳️ Gem Harbor - Retention Rate D3 Root Cause Analysis
**iKame EF2026 | Capstone Project**

---

## Cấu trúc dự án

```
notebooks/
├── eda.ipynb           # Notebook phân tích chính
├── .env                # Biến môi trường (PROJECT_ID, credentials)
├── .gitignore
├── README.md
└── requirements.txt    # Danh sách thư viện
```

---

## Thư viện yêu cầu

```txt
google-cloud-bigquery
pandas
numpy
matplotlib
seaborn
python-dotenv
db-dtypes
```

> Xem đầy đủ tại `requirements.txt`.

---

## Cài đặt môi trường với `uv`

### 1. Cài `uv` (nếu chưa có)

```bash
pip install uv
```

### 2. Tạo Virtual Environment

```bash
uv venv myenv --python 3.13.14
```

### 3. Kích hoạt Virtual Environment

**macOS / Linux:**
```bash
source myenv/bin/activate
```

**Windows:**
```bash
myenv\Scripts\activate
```

### 4. Cài đặt thư viện

```bash
uv pip install -r requirements.txt
```

### 5. Cấu hình `.env`

Tạo file `.env` tại thư mục gốc với nội dung:

```env
PROJECT_ID= -- ID của dự án.
```

---

## Câu hỏi phân tích chính

### Tìm ra nguyên nhân cốt lõi khiến RR D3 sụt, chứng minh bằng dữ liệu (loại trừ các giả thuyết sai), và đưa ra khuyến nghị cho BU.

**Nguyên nhân cốt lõi** khiến tỷ lệ giữ chân ngày thứ 3 (RR D3) của Gem Harbor sụt giảm là sự kết hợp của hai quyết định thay đổi trong vận hành campaign diễn ra từ ngày 23/03 đến 24/03/2026, cụ thể:

**1. UA hạ ROAS goal trên các campaign hiện có:**
Việc giảm mục tiêu hiệu quả doanh thu đã khiến thuật toán của mạng quảng cáo ưu tiên tìm kiếm những lượt cài đặt giá rẻ với số lượng lớn, thay vì tập trung vào nhóm người dùng chất lượng cao như trước.

**2. UA tăng mạnh ngân sách cùng một thời điểm:**
Khi lượng tiền đổ vào quảng cáo tăng vọt từ ngày 24/03, hệ thống buộc phải mở rộng phạm vi hiển thị một cách nhanh chóng để tiêu hết ngân sách.

**Hệ quả - Pha loãng chất lượng người dùng (Audience Dilution):**
Sự kết hợp này đã mang về một lượng lớn người chơi mới nhưng đây lại là tệp người dùng đại trà, có mức độ gắn kết thấp với trò chơi. Chính vì vậy, tỷ lệ giữ chân D3 đã bị bẻ gãy ngay từ nhóm người dùng cài đặt game từ ngày 24-25/03 trở đi.

---

### Các giả thuyết đã được loại trừ

| Giả thuyết | Lý do loại trừ |
|---|---|
| **A/B Test gây sụt RR** | Control group (không nhận thay đổi sản phẩm) cũng sụt giảm cùng lúc với Treatment group - chứng tỏ vấn đề không nằm ở design sản phẩm |
| **Version/App Update gây lỗi** | Crash/ANR tồn tại từ trước như một vấn đề nền, không có biến động đột ngột tại mốc 24/03 khớp với breakpoint của RR |
| **Crash/ANR trực tiếp gây sụt RR lần này** | Product đã nỗ lực giảm Crash/ANR qua các bản update mới nhưng RR vẫn tiếp tục giảm - chứng tỏ đây là hai vấn đề độc lập |
| **Game content D3–D4 có vấn đề** | Product confirm user test qua các level vẫn ổn, segment không thay đổi, không có breakpoint content nào tại 24/03 |
| **Mở rộng Tier 2/3 là nguyên nhân duy nhất** | Khi bóc tách riêng thị trường US (Tier 1), RR vẫn sụt |

---

### Khuyến nghị cho BU
- Rollback ROAS goal về mức trước ngày 23/03 trên tất cả campaign/country bị Config Change mà không đem lại hiệu quả RR, với những campaign/country vẫn giúp tăng RR thì không thay đổi
- Không nên đồng thời hạ ROAS goal VÀ tăng spend trong cùng một ngày
- Khi muốn scale spend, hãy đợi RR D3 ổn định liên tiếp ít nhất 5 ngày sau khi thay đổi ROAS goal
- Chờ đến khi UA đã rollback và RR D3 ổn định trở lại
- Bên khối kỹ thuật tiếp tục cải thiện tình trạng Crash/ANR