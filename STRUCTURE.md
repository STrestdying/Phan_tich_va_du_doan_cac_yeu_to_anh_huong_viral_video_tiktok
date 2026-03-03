# Cấu trúc Thư mục Dự án

## 📁 Tổng quan

```
Phan_tich_va_du_doan_cac_yeu_to_anh_huong_viral_video_tiktok/
│
├── 📄 README.md                    # ⭐ Hướng dẫn dự án chính
├── 📄 CONTRIBUTING.md              # Hướng dẫn đóng góp
├── 📄 PROJECT_CONFIG.md            # Cấu hình & ghi chú kỹ thuật
├── 📄 requirements.txt              # Python dependencies
├── 📄 .gitignore                   # Git ignore patterns
│
├── 📁 notebooks/                   # Jupyter Notebooks
│   └── 📔 ax02.ipynb              # Main analysis workflow
│
├── 📁 data/                        # Input data
│   └── .gitkeep                   # (placeholder, actual .parquet files go here)
│
├── 📁 outputs/                     # Generated outputs
│   ├── 📊 feature_importance.png                # Feature ranking
│   ├── 📊 shap_summary.png                     # SHAP interpretation
│   ├── 📊 shap_waterfall.png                   # Individual prediction
│   │
│   ├── 📊 q1_play_distribution.png             # Q1: Play count distribution
│   ├── 📊 q2_duration.png                      # Q2: Duration impact
│   ├── 📊 q3_verified_ad.png                   # Q3: Verified & Ad
│   ├── 📊 q4_correlation.png                   # Q4: Correlation matrix
│   ├── 📊 q5_duration_linear.png               # Q5: Duration linearity
│   │
│   ├── 📊 q_music_duration.png                 # Q: Music duration
│   ├── 📊 q_music_original.png                 # Q: Original vs trending
│   ├── 📊 q_music_original_deep.png            # Q: Deep analysis
│   ├── 📊 q_music_viral.png                    # Q: Viral music
│   ├── 📊 q_playlist.png                       # Q: Playlist impact
│   │
│   ├── 📊 q_duet_stitch.png                    # Q: Duet & Stitch
│   ├── 📊 q_poi_category.png                   # Q: POI categories
│   ├── 📊 q_poi_engagement.png                 # Q: POI engagement
│   │
│   ├── 📊 q_seller_verified.png                # Q: User segments
│   ├── 📊 q_city.png                           # Q: City analysis
│   ├── 📊 q_country.png                        # Q: Country analysis
│   │
│   ├── 📊 q_hourly.png                         # Q: Hourly trends
│   ├── 📊 q_daily.png                          # Q: Daily trends
│   └── 📊 q_heatmap_hour_day.png               # Q: Heatmap hour×day
│
└── 📁 src/                         # Source code & utilities
    └── __init__.py                # Package marker
```

---

## 📊 Chi tiết từng thư mục

### `notebooks/`
**Mục đích**: Chứa Jupyter Notebooks

**File chính**:
- `ax02.ipynb` - Notebook chính với 88 cells:
  - Cells 1-2: Library imports
  - Cells 3-14: Data cleaning
  - Cells 15-54: EDA (58 visualizations)
  - Cells 55-63: Feature engineering (46 features)
  - Cells 64-72: Model training & evaluation
  - Cells 73-76: SHAP interpretation

### `data/`
**Mục đích**: Input data storage

**Được ignore**: `.parquet` files
**Tracked**: `.gitkeep` (giữ thư mục empty trong git)

**Cách thêm data**:
```bash
# Place your parquet files here
data/*.parquet
```

### `outputs/`
**Mục đích**: Generated visualizations & results

**File types**:
- `.png` - Matplotlib/Seaborn plots (24 EDA + 3 model plots)
- Sau mỗi notebook run, tất cả plots được save ở đây

**Được ignore**: Tất cả `.png` files (to save space)

### `src/`
**Mục đích**: Reusable Python modules

**Current**: Placeholder cho future utility functions
**Future**: 
```python
src/
├── __init__.py
├── data_preprocessing.py   # Data cleaning functions
├── eda_utils.py           # EDA helper functions
├── feature_engineering.py # Feature creation
└── model_utils.py         # Model training utilities
```

---

## 📋 Configuration Files

### `requirements.txt`
Chi tiết của tất cả Python packages cần thiết:
```
pandas>=1.3.0
numpy>=1.21.0
scikit-learn>=1.0.0
xgboost>=1.5.0
shap>=0.41.0
matplotlib>=3.4.0
seaborn>=0.11.0
scipy>=1.7.0
jupyter>=1.0.0
```

**Cài đặt**:
```bash
pip install -r requirements.txt
```

### `.gitignore`
Quy tắc ignore cho git:
- `__pycache__/` - Python cache
- `.ipynb_checkpoints/` - Notebook checkpoints
- `data/*.parquet` - Large data files
- `outputs/*.png` - Generated plots
- `.venv/`, `venv/` - Virtual environments
- `.DS_Store`, `Thumbs.db` - OS files

### `README.md` (Main Documentation)
- ⭐ **START HERE**
- Project overview
- 25+ research questions
- Technology stack
- Model results & insights
- Creator recommendations
- Setup instructions

### `CONTRIBUTING.md`
- Workflow guidelines
- Project structure explanation
- Useful commands
- Troubleshooting
- Contact info

### `PROJECT_CONFIG.md`
- Dataset information & columns
- Processing pipeline details
- Key metrics & thresholds
- Output artifacts
- Hyperparameters
- Performance baselines
- Next steps / TODO

---

## 🚀 Cách Sử dụng

### 1️⃣ Clone & Setup
```bash
git clone <repo-url>
cd Phan_tich_va_du_doan_cac_yeu_to_anh_huong_viral_video_tiktok

# Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

### 2️⃣ Chuẩn bị Data
```bash
# Put your .parquet files in data/
# Example:
# data/tiktok_videos_part1.parquet
# data/tiktok_videos_part2.parquet
```

### 3️⃣ Run Notebook
```bash
jupyter notebook notebooks/ax02.ipynb

# Hoặc từ VSCode:
# Ctrl+Shift+P → Jupyter: Create New Blank Notebook
```

### 4️⃣ View Results
```bash
# Outputs automatically saved to:
# outputs/*.png
```

### 5️⃣ Commit to Git
```bash
# Only tracked files are committed
git add .
git commit -m "feat: complete EDA analysis"
git push origin main
```

---

## 📌 Important Notes

### ✅ Tracked in Git
- ✓ `.ipynb` (notebook)
- ✓ `.md` (documentation)
- ✓ `.py` (source code)
- ✓ `requirements.txt`
- ✓ `.gitignore`
- ✓ `.gitkeep` (empty dir tracking)

### ❌ NOT Tracked in Git
- ✗ `data/*.parquet` (large files)
- ✗ `outputs/*.png` (generated plots)
- ✗ `__pycache__/` (Python cache)
- ✗ `.ipynb_checkpoints/`
- ✗ `.venv/` (virtual env)

---

## 🎯 Best Practices

1. **Before Committing Notebooks**:
   ```bash
   # Clear outputs
   jupyter nbconvert --ClearOutputPreprocessor.enabled=True \
     --inplace notebooks/ax02.ipynb
   ```

2. **Update Requirements**:
   ```bash
   # After installing new package
   pip freeze > requirements.txt
   ```

3. **Commit Messages**:
   ```
   feat: add SHAP analysis
   fix: correct feature engineering bug
   docs: update README with results
   refactor: optimize EDA code
   ```

4. **Data Handling**:
   - Never commit `.parquet` files
   - Document data source in `PROJECT_CONFIG.md`
   - Use `.gitkeep` for empty directories

---

**Last Updated**: March 2025  
**Project Version**: 1.0.0
