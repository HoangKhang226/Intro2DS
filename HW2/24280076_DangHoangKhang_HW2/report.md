# BÁO CÁO BÀI TẬP HW2
## Xây dựng mô hình Machine Learning — Heart Disease Classification

**Môn học:** Nhập môn Khoa học Dữ liệu  
**MSSV:** 24280076  
**Họ tên:** Đặng Hoàng Khang

---

## 1. Giới thiệu bài toán

Bệnh tim mạch là nguyên nhân gây tử vong hàng đầu trên thế giới. Bài toán đặt ra là **phân loại nhị phân**: dựa trên các chỉ số sinh học và lâm sàng để dự đoán bệnh nhân có mắc bệnh tim hay không.

- **Biến mục tiêu:** `target` (1 = Có bệnh, 0 = Không bệnh)

---

## 2. Mô tả dataset

Bộ dữ liệu `heart_disease_dataset.csv` gồm **1025 mẫu** và **14 cột**:

| Biến | Ý nghĩa |
|---|---|
| age | Tuổi |
| sex | Giới tính (1=Nam, 0=Nữ) |
| cp | Loại đau ngực (0–3) |
| trestbps | Huyết áp lúc nghỉ (mmHg) |
| chol | Cholesterol huyết thanh (mg/dl) |
| fbs | Đường huyết lúc đói > 120 mg/dl |
| restecg | Kết quả ECG lúc nghỉ |
| thalach | Nhịp tim tối đa |
| exang | Đau ngực khi gắng sức |
| oldpeak | ST depression so với lúc nghỉ |
| slope | Độ dốc đoạn ST |
| ca | Số mạch máu chính (0–4) |
| thal | Thalassemia (0–3) |
| target | Bệnh tim (1=Có, 0=Không) |

---

## 3. Khám phá dữ liệu (EDA)

### 3.1 Chất lượng dữ liệu
- **Không có giá trị thiếu** (missing value)
- Có **723 dòng trùng lặp** (chiếm 70.5%) — cần loại bỏ trước khi huấn luyện mô hình

### 3.2 Phân phối biến mục tiêu
- target=1 (Có bệnh): 526 mẫu (**51.3%**)
- target=0 (Không bệnh): 499 mẫu (**48.7%**)
- Dữ liệu gần như cân bằng giữa 2 lớp

### 3.3 Phân tích biến số
- `thalach`: người có bệnh tim có nhịp tim max **cao hơn** (~19 bpm so với nhóm không bệnh)
- `oldpeak`: người có bệnh tim có giá trị **thấp hơn** rõ rệt
- `age`: người không bệnh có tuổi trung bình cao hơn (~4 năm)
- `chol`, `trestbps`: sự khác biệt giữa 2 nhóm không quá rõ

### 3.4 Phân tích biến phân loại
- `sex`: Nữ (sex=0) có tỉ lệ bệnh tim **cao hơn rất nhiều** (~75%) so với nam (~45%)
- `cp`: cp=0 có tỉ lệ bệnh thấp nhất (~27%); cp=1,2 rất cao (>80%)
- `exang`: Người không đau ngực khi gắng sức lại có tỉ lệ bệnh cao hơn (~70%)
- `ca`: ca=0 có tỉ lệ bệnh rất cao (~74%); ca tăng thì tỉ lệ giảm
- `thal`: thal=2 có tỉ lệ bệnh cao nhất (~78%)

### 3.5 Tương quan với target
- Tương quan mạnh nhất: `exang` (-0.44), `cp` (+0.43), `oldpeak` (-0.43), `thalach` (+0.42), `ca` (-0.41)
- Tương quan yếu: `chol` (-0.08), `fbs` (-0.03)

### 3.6 Outlier
- `trestbps`: 9 outliers (3.0%)
- `chol`: 5 outliers (1.7%)
- `oldpeak`: 5 outliers (1.7%)

---

## 4. Tiền xử lý dữ liệu

1. **Loại bỏ trùng lặp:** 1025 → **302 mẫu** (loại 723 dòng trùng)
2. **Xử lý outlier:** Capping IQR cho `chol`, `trestbps`, `oldpeak`
3. **Chia train/test:** 80/20 với `stratify=y` → Train: 241, Test: 61
4. **Chuẩn hóa:** StandardScaler (fit trên train, transform trên test)

---

## 5. Mô hình sử dụng

1. **Logistic Regression** — mô hình tuyến tính, dễ giải thích
2. **KNN** (k=5) — phân loại dựa trên khoảng cách
3. **Decision Tree** — học các quy tắc phi tuyến
4. **Random Forest** (100 cây) — ensemble giảm overfitting
5. **SVM** (RBF kernel) — tối ưu biên phân tách

*Logistic Regression, KNN, SVM dùng dữ liệu đã scale; Decision Tree, Random Forest dùng dữ liệu gốc.*

---

## 6. Kết quả đánh giá

| Mô hình | Accuracy | Precision | Recall | F1-score | ROC-AUC | Train Acc |
|:---|:---:|:---:|:---:|:---:|:---:|:---:|
| **Logistic Regression** | **0.8033** | 0.8000 | **0.8485** | **0.8235** | **0.8723** | 0.8548 |
| **KNN** | **0.8033** | 0.8000 | **0.8485** | **0.8235** | 0.8469 | 0.8921 |
| **Decision Tree** | 0.8033 | **0.8182** | 0.8182 | 0.8182 | 0.8019 | **1.0000** |
| **SVM** | 0.7705 | 0.7714 | 0.8182 | 0.7941 | 0.8366 | 0.9212 |
| **Random Forest** | 0.7541 | 0.7647 | 0.7879 | 0.7761 | 0.8647 | **1.0000** |

---

## 7. So sánh và phân tích

### Mô hình tốt nhất
- **Logistic Regression** và **KNN** cùng đạt F1-score cao nhất (0.8235) và Recall cao nhất (0.8485)
- Logistic Regression có **ROC-AUC cao nhất (0.8723)** → được chọn là mô hình tối ưu

### Overfitting / Underfitting
- **Decision Tree** và **Random Forest**: Train Accuracy = 100% nhưng Test chỉ ~75-80% → **overfitting rõ ràng**
- **Logistic Regression**: Train 85.5% vs Test 80.3% → **cân bằng tốt**, ít overfitting nhất
- **KNN**: Train 89.2% vs Test 80.3% → hơi overfitting nhưng chấp nhận được

### Tại sao Logistic Regression tốt nhất?
- Dữ liệu sau dedup chỉ còn **302 mẫu** — quá ít cho mô hình phức tạp
- Mối quan hệ giữa các biến và target có tính **tương đối tuyến tính**
- Mô hình đơn giản tránh overfitting trên tập nhỏ

### Feature Importance (Random Forest)
- Top 5: `cp` (0.174), `thalach` (0.132), `ca` (0.105), `oldpeak` (0.096), `thal` (0.091)
- Phù hợp với phân tích tương quan ở EDA

---

## 8. Kết luận và hướng cải thiện

### Kết luận
- **Logistic Regression** là mô hình tối ưu: Accuracy = 80.3%, Recall = 84.9%, ROC-AUC = 0.8723
- Dữ liệu có tỉ lệ trùng lặp rất cao (70.5%), việc loại bỏ trùng lặp giúp đánh giá mô hình chính xác hơn
- Các biến quan trọng nhất: `cp`, `thalach`, `ca`, `oldpeak`, `thal`

### Hướng cải thiện
1. Tinh chỉnh hyperparameter bằng GridSearchCV
2. Áp dụng Cross-validation (k-fold) để đánh giá ổn định hơn
3. Giới hạn `max_depth` cho Decision Tree/Random Forest để giảm overfitting
4. Thu thập thêm dữ liệu sạch
5. Thử feature engineering: tạo biến tương tác giữa các chỉ số sinh học
