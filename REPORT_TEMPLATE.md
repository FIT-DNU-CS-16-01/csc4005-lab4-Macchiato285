# CSC4005 Lab 4 Report – CRNN for UrbanSound8K

## 1. Thông tin sinh viên

- Họ tên: Ngô Nguyễn Quang Linh 
- Mã sinh viên:1671040018
- Lớp:KHMT 16-01
- Link GitHub repo:https://github.com/FIT-DNU-CS-16-01/csc4005-lab4-Macchiato285
- Link W&B project:https://wandb.ai/ngoquanglinh2853-none/csc4005-lab4-urbansound8k-crnn?nw=nwuserngoquanglinh2853

## 2. Mục tiêu thí nghiệm

Lab 4 tập trung xây dựng mô hình CRNN cho bài toán phân loại âm thanh môi trường trên tập UrbanSound8K. Dữ liệu audio được chuyển sang log-mel spectrogram để giữ được cấu trúc thời gian–tần số, giúp mô hình học hiệu quả hơn. So với 1D-CNN ở Lab 3, CRNN bổ sung thêm GRU/LSTM để học quan hệ theo chuỗi thời gian thay vì chỉ học pattern cục bộ. Mục tiêu chính của bài lab là đánh giá khả năng phân loại âm thanh của CRNN thông qua accuracy, learning curves và confusion matrix.


## 3. Cấu hình dữ liệu

| Thành phần | Giá trị |
|---|---|
| Dataset | UrbanSound8K |
| Số lớp | 10 |
| Train folds | 1–8 |
| Validation fold | 9 |
| Test fold | 10 |
| Feature | log-mel spectrogram |
| Sampling rate | 16 kHz |
| Duration | 4 giây |

## 4. Cấu hình mô hình

| Thành phần | Giá trị |
|---|---|
| Model | CRNN |
| CNN blocks | 2 convolution blocks |
| RNN type | GRU / LSTM |
| Hidden size | 128 |
| Dropout | 0.3 |
| Optimizer | AdamW|
| Learning rate | 0.001 |
| Batch size | 32 |
| Epochs | 25 |

## 5. Kết quả huấn luyện

Điền kết quả tốt nhất từ W&B hoặc `metrics.json`.

| Run | best_val_acc | test_acc | Ghi chú |
|---|---:|---:|---|
| logmel_crnn_gru_baseline | 0.74 | 0.72 | Validation accuracy tăng ổn định |
| extension_bilstm_crnn | chưa chạy | chưa chạy | Không thực hiện extension |

## 6. Learning curves

Chèn hình `curves.png`.

Nhận xét:

- Train loss và validation loss đều giảm theo epoch, cho thấy mô hình học được đặc trưng hữu ích từ dữ liệu.
- Validation accuracy tăng dần từ khoảng 0.32 lên khoảng 0.74, quá trình huấn luyện tương đối ổn định.
- Khoảng cách giữa train accuracy và validation accuracy không quá lớn nên chưa xuất hiện overfitting nghiêm trọng.
- Validation loss giảm tương đối đều và không tăng mạnh ở cuối quá trình train.
- Early stopping vẫn cần thiết để lưu lại mô hình có validation loss tốt nhất và tránh overfit khi train lâu hơn.
## 7. Confusion matrix

Chèn hình `confusion_matrix.png`.

Nhận xét:

- Các lớp có âm thanh đặc trưng mạnh như siren và gun_shot thường được phân loại tốt hơn.
- Một số lớp dễ bị nhầm như drilling và jackhammer vì đều là âm thanh cơ khí có tần số và năng lượng tương đối giống nhau.
- children_playing đôi khi bị nhầm với street_music do background noise phức tạp và có nhiều âm thanh hỗn hợp.
- Các lỗi nhầm lẫn nhìn chung hợp lý về mặt đặc điểm âm thanh thực tế.
## 8. So sánh với Lab 3 1D-CNN

| Tiêu chí | Lab 3: 1D-CNN | Lab 4: CRNN |
|---|---|---|
| Feature chính | MFCC / log-mel | log-mel |
| Khả năng học pattern cục bộ | Có | Có |
| Khả năng học quan hệ thời gian | Hạn chế | Tốt hơn |
| Test accuracy | ~0.68 | ~0.72 |
| Nhận xét | Huấn luyện nhanh hơn nhưng khó học temporal pattern | Accuracy cao hơn và ổn định hơn |

## 9. Kết luận

- Mô hình CRNN trong Lab 4 cho kết quả tốt hơn so với 1D-CNN ở Lab 3 nhờ khả năng học cả đặc trưng cục bộ và quan hệ thời gian của âm thanh. Validation accuracy tăng ổn định và validation loss giảm đều trong quá trình train, cho thấy mô hình học tương đối hiệu quả. Tuy vẫn còn một số lớp dễ bị nhầm lẫn, kết quả tổng thể của CRNN là khá tốt với tập UrbanSound8K. So với 1D-CNN, CRNN cần thời gian train lâu hơn nhưng đổi lại khả năng tổng quát hóa tốt hơn. Nếu tiếp tục cải thiện mô hình, có thể thử BiLSTM-CRNN, tăng cường augmentation hoặc tuning thêm learning rate và hidden size.

## 10. Link minh chứng

- GitHub commit cuối:https://github.com/FIT-DNU-CS-16-01/csc4005-lab4-Macchiato285
- W&B run baseline:https://wandb.ai/ngoquanglinh2853-none/csc4005-lab4-urbansound8k-crnn?nw=nwuserngoquanglinh2853
- W&B run mở rộng:
