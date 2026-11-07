# Báo cáo cá nhân — Day 01: Lọc tin nhắn rác bằng ML, DL, Embedding và LLM

> Đổi tên file này thành `MSSV_HoTen.md` của bạn, ví dụ `2A20260000_NguyenVanAn.md`, rồi điền từng mục. Xoá các dòng hướng dẫn in nghiêng khi xong.

- **MSSV:** 2A202604108
- **Họ và tên:** Nguyễn Ngọc bách
- **SEED** (in ở ô đầu notebook): 8053

## 1. Bằng chứng

| Tệp | Có? |
| --- | --- |
| [`day01_lab.ipynb`](day01_lab.ipynb) — notebook đã chạy, còn output | [X] |
| [`lab1_report.json`](lab1_report.json) — do ô "Báo cáo cuối" sinh ra | [x] |
| [`hand_calc.jpg`](hand_calc.jpg) — ảnh tính tay có các bước | [x] |

## 2. Kết quả chính

*Chép bảng so sánh cuối Phần 5 (F1 test, F1 SUB 40, p95 ms/tin, USD/1k tin) của chính bạn.*

                                F1 test thường (SUB)  F1 50 case khó  \
1. BoW + LogReg                                0.889           0.308   
2. BoW + MLP                                   0.919           0.350   
3a. Emb + kNN                                  0.976           0.769   
3b. Emb + LogReg                               1.000           0.643   
3c. Emb + MLP head                             1.000           0.702   
4a. LLM nhỏ zero-shot                          0.927           0.985   
4d. LLM nhỏ + prompt phòng thủ                 0.927           1.000   

                                acc 50 case khó  
1. BoW + LogReg                           0.509  
2. BoW + MLP                              0.527  
3a. Emb + kNN                             0.727  
3b. Emb + LogReg                          0.636  
3c. Emb + MLP head                        0.691  
4a. LLM nhỏ zero-shot                     0.982  
4d. LLM nhỏ + prompt phòng thủ            1.000 

## 3. Dự đoán và thực tế

predict("llm_fewshot", "F1 sẽ tăng, mỗi tin tăng khoảng 4 lần") 
Fewshot không làm F1 tăng 

## 4. Phân tích 50 case khó

*Model nào tụt nhiều nhất, ở nhóm case nào? Nêu 2 case cụ thể và cơ chế khiến model sai.*

## 5. Thử thách cá nhân

Đề #2: Ảnh hưởng của kích thước vocabulary đến F1

**Cách làm:** 

Tôi thay đổi kích thước vocab thành 100 và 5000 từ, sau đó train lại Logistic Regression với cùng learning rate = 0.5 và 300 epochs, rồi đánh giá trên tập test.

**Kết quả:**

Vocab 100: F1 = 0.8539

Vocab 5000: F1 = 0.9081

**Giải thích:**

 Vocab 5000 cho F1 cao hơn vì giữ được nhiều từ quan trọng, giúp mô hình nhận diện spam tốt hơn. Vocab 100 làm mất nhiều thông tin nên kết quả thấp hơn. Tuy nhiên, vocab quá lớn vẫn có thể gây nhiễu và overfitting.

## 6. Phương án cho sản phẩm thật

*Với yêu cầu p95 < 50 ms/tin cho 10 triệu SMS/ngày: chọn phương án nào, dẫn số liệu nào, chấp nhận đánh đổi gì.*

Tôi chọn MiniLM + MLP head vì trong thử nghiệm mô hình đạt F1 = 1.0 trên SUB và p95 dưới 50 ms, phù hợp hơn LLM với yêu cầu xử lý 10 triệu SMS/ngày (~116 tin/giây). 

**Đánh đổi:** Chấp nhận tăng độ phức tạp hệ thống và chi phí để cải thiện chất lượng

## 7. AI log và điều đã học

*Mỗi lần dùng AI: hỏi gì, nhận gì, bạn sửa gì. Sau đó 2–3 câu: điều bạn hiểu ra hôm nay mà trước đó chưa hiểu.*

Tôi  hiểu rằng khi triển khai AI thực tế, cần cân bằng độ chính xác, tốc độ và chi phí thay vì chỉ chọn mô hình có F1 cao nhất.