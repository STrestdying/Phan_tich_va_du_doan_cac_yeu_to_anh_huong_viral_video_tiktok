# Hướng dẫn Đóng góp

## Quy trình Workflow

### 1. Clone Repository
```bash
git clone <repo-url>
cd Phan_tich_va_du_doan_cac_yeu_to_anh_huong_viral_video_tiktok
```

### 2. Cài đặt Dependencies
```bash
pip install -r requirements.txt
```

### 3. Chuẩn bị Dữ liệu
- Đặt file `.parquet` vào thư mục `data/`
- Đảm bảo file có tên và cấu trúc phù hợp

### 4. Chạy Notebook
```bash
jupyter notebook notebooks/ax02.ipynb
```

### 5. Kết quả Output
- Tất cả visualization (.png) sẽ lưu vào `outputs/`
- Tất cả intermediate results sẽ print trong notebook

---

## Cấu trúc Thư mục

```
.
├── README.md                    # Project documentation
├── CONTRIBUTING.md              # This file
├── requirements.txt             # Python dependencies
├── .gitignore                   # Git ignore patterns
│
├── notebooks/
│   └── ax02.ipynb              # Main analysis notebook
│
├── data/
│   ├── .gitkeep               # Keep empty dir in git
│   └── *.parquet              # Input data files (not in git)
│
├── outputs/
│   ├── .gitkeep               # Keep empty dir in git
│   └── *.png                  # Generated visualizations
│
└── src/
    └── __init__.py            # Python package marker
```

---

## Các Lệnh Hữu ích

### Cài đặt từ requirements
```bash
pip install -r requirements.txt
```

### Update requirements từ environment
```bash
pip freeze > requirements.txt
```

### Chạy notebook từ terminal
```bash
jupyter notebook
```

### Chạy từng cell của notebook
```bash
jupyter nbconvert --to notebook --execute notebooks/ax02.ipynb
```

---

## Lưu ý Quan trọng

1. **Data files**: File `.parquet` quá lớn, không nên push lên git. Đã có trong `.gitignore`
2. **Outputs**: File `.png` (visualization) cũng không push lên git
3. **Notebook**: Nên clean notebook trước khi commit (xóa outputs của cells)
4. **Commit message**: Viết clear, vd: `feat: add SHAP analysis for feature importance`

---

## Troubleshooting

### ImportError: No module named 'xgboost'
```bash
pip install xgboost>=1.5.0
```

### ImportError: No module named 'shap'
```bash
pip install shap>=0.41.0
```

### Notebook quá chậm khi chạy EDA
- Chạy từng section riêng lẻ thay vì toàn bộ
- Sử dụng sample data nhỏ để test

### Memory error
- Giảm sample size khi exploratory analysis
- Sử dụng chunk processing cho large datasets

---

## Contact
Liên hệ cho issues, questions, hoặc suggestions
