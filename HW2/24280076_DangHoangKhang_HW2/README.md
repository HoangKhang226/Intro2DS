# 24280076 - Đặng Hoàng Khang - HW2

## Hướng dẫn chạy

### Yêu cầu môi trường
- Python 3.11
- Các thư viện trong `requirements.txt`

### Cài đặt

```bash
# Tạo môi trường ảo
python -m venv venv

# Kích hoạt (Windows)
.\venv\Scripts\Activate.ps1

# Cài thư viện
pip install -r requirements.txt
```

### Chạy notebook

```bash
jupyter notebook notebook.ipynb
```

Chọn kernel **Python 3 (HW2 venv)** và chạy toàn bộ cells.

### Cấu trúc thư mục

```
24280076_DangHoangKhang_HW2/
├── README.md
├── report.md
├── notebook.ipynb
├── requirements.txt
├── data/
│   └── heart_disease_dataset.csv
└── outputs/
    ├── figures/       ← Biểu đồ tự động lưu khi chạy notebook
    └── results.csv    ← Bảng kết quả các mô hình
```

### Bài toán
- **Dataset:** Heart Disease Dataset
- **Bài toán:** Classification — Dự đoán bệnh nhân có mắc bệnh tim hay không
- **Biến mục tiêu:** `target` (1 = Có bệnh tim, 0 = Không có bệnh tim)
- **Mô hình:** Logistic Regression, KNN, Decision Tree, Random Forest, SVM
