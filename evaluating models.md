# 📊 Báo Cáo Đánh Giá Mô Hình Nhận Dạng Giọng Nói (ASR)
> **Dự án:** Meeting Speech-to-Summary  
> **Tập dữ liệu kiểm thử:** VIVOS Dataset (10 mẫu ngẫu nhiên từ tập DEV)  
> **Chỉ số đánh giá:** Word Error Rate (WER)

---

## 📌 1. Bảng Tổng Hợp Kết Quả (Summary)

| Mô hình | Nhà phát triển | Số lượng mẫu | Word Error Rate (WER) | Đánh giá tiếng Việt |
| :--- | :--- | :---: | :---: | :--- |
| **`vinai/PhoWhisper-small`** | VinAI Research | 10 | **0.00%** 🏆 | Nhận diện cực kỳ chính xác dấu thanh, từ vựng và ngữ cảnh tiếng Việt. |
| **`openai/whisper-small`** | OpenAI | 10 | **30.47%** | Thường xuyên nhầm lẫn dấu thanh, âm vị tương đồng và từ ghép. |

---

## 🔍 2. So Sánh Chi Tiết Từng Mẫu (Sample Comparison)

### 🔹 Mẫu 1: `VIVOSDEV19_196`
* **Chuẩn (Ground Truth):**
  > `người anh sau đó được phép thầu xây kênh này nhưng không tiến hành`
* **openai/whisper-small:**
  > `người ăn sau đó được phép thầu xây kênh này nhưng không tiếng hành.` *(Sai: anh ➔ ăn, tiến ➔ tiếng)*
* **vinai/PhoWhisper-small:**
  > `người anh sau đó được phép thầu xây kênh này nhưng không tiến hành` ✅

---

### 🔹 Mẫu 2: `VIVOSDEV19_020`
* **Chuẩn (Ground Truth):**
  > `lên các cuộc hẹn phỏng vấn và thực hiện phỏng vấn`
* **openai/whisper-small:**
  > `lên các cuộc hẹn phong phóng và thực hiện phóng phóng.` *(Sai lặp từ và sai thanh điệu nặng)*
* **vinai/PhoWhisper-small:**
  > `lên các cuộc hẹn phỏng vấn và thực hiện phỏng vấn` ✅

---

### 🔹 Mẫu 3: `VIVOSDEV19_030`
* **Chuẩn (Ground Truth):**
  > `phù sa bồi đắp ruộng vườn cho xóm làng trù phú`
* **openai/whisper-small:**
  > `phụ xe bò đắp rụng vườn cho sớm làn trụ phú.` *(Sai hầu hết các từ quan trọng)*
* **vinai/PhoWhisper-small:**
  > `phù sa bồi đắp ruộng vườn cho xóm làng trù phú` ✅

---

### 🔹 ... Các mẫu trung gian ...

---

### 🔹 Mẫu 10: `VIVOSDEV19_222`
* **Chuẩn (Ground Truth):**
  > `mắt tự nhiên hoa lên trí chụp một cái ly quật mạnh xuống sàn`
* **openai/whisper-small:**
  > `mắt tự nhiên hoa lên trí chụp một cái ly hoặc mạnh xuống sàn.` *(Sai: quật ➔ hoặc)*
* **vinai/PhoWhisper-small:**
  > `mắt tự nhiên hoa lên trí chụp một cái ly quật mạnh xuống sàn` ✅

---

## 💡 3. Phân Tích & Nhận Xét

### 1. Mô hình `openai/whisper-small` (Đa ngôn ngữ):
* **Ưu điểm:** Khả năng nhận diện đa ngôn ngữ, kiến trúc tổng quát tốt.
* **Nhược điểm với tiếng Việt:**
  * Dễ sai lệch hệ thống thanh điệu (*hỏi/ngã/sắc/nặng/huyền*).
  * Gặp khó khăn khi giải mã các từ vựng mang tính tượng hình, ngữ cảnh văn phong Việt Nam.
  * Tỷ lệ lỗi **WER = 30.47%** là tương đối cao cho tác vụ bóc băng cuộc họp yêu cầu độ chính xác về mặt nội dung và tên riêng.

### 2. Mô hình `vinai/PhoWhisper-small` (Chuyên biệt tiếng Việt):
* **Ưu điểm vượt trội:**
  * Được tiền huấn luyện và tinh chỉnh chuyên sâu trên các tập dữ liệu giọng nói tiếng Việt quy mô lớn.
  * Bắt đúng ngữ điệu địa phương, ngữ cảnh ngữ nghĩa tiếng Việt.
  * Đạt tỷ lệ lỗi **WER = 0.00%** trên tập mẫu thử nghiệm, không gặp hiện tượng hallucination hay mất dấu.

---

## 🎯 4. Kết Luận & Đề Xuất

> **Khuyến nghị:** Sử dụng dòng mô hình **`vinai/PhoWhisper`** (`PhoWhisper-small` hoặc `PhoWhisper-base`/`large` tùy tài nguyên tính toán) làm module **ASR (Automatic Speech Recognition)** chính trong pipeline **Meeting Speech-to-Summary** để đảm bảo chất lượng văn bản đầu vào tốt nhất cho giai đoạn tóm tắt văn bản (LLM Summarization).
