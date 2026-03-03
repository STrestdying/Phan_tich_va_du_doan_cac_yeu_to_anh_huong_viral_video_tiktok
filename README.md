# Phân tích & Dự đoán Các Yếu tố Ảnh hưởng đến Viral Video TikTok

## 📋 Tổng quan dự án

Dự án này thực hiện **phân tích dữ liệu toàn diện (EDA)** và **xây dựng mô hình học máy** để:
1. **Hiểu rõ các yếu tố** ảnh hưởng đến việc video trở nên viral trên TikTok
2. **Dự đoán lượt xem** (play_count) của video mới dựa trên các đặc trưng
3. **Phân loại** video thành `Viral` hoặc `Non-viral` (top 15% theo Viral Score)
4. **Cung cấp insights hành động** để creators tối ưu hóa nội dung

---

## 🎯 Câu hỏi Nghiên cứu Chính

### Phần I: Phân tích Tổng quan
- **Q1**: Play count có bị lệch phải không? → Cần log transformation
- **Q2**: Duration nào hiệu quả nhất? → 15-60s tối ưu
- **Q3**: Verified & Ad ảnh hưởng bao nhiêu % đến engagement?
- **Q4**: Ma trận tương quan giữa các chỉ số engagement
- **Q5**: Duration có tương quan tuyến tính với play_count không?

### Phần II: Phân tích Nội dung & Tính năng
- **Q1**: Music Original vs Non-original → Nhạc trending viral hơn
- **Q2**: Playlist có giúp tăng lượt xem không?
- **Q3**: Duet & Stitch ảnh hưởng đến tương tác không?
- **Q4**: POI Category nào có tỷ lệ viral cao nhất?

### Phần III: Phân tích Người dùng
- **Q1 & Q2**: TikTok Seller & Verified badge ảnh hưởng như thế nào?
- **Q3**: Có sự khác biệt hiệu suất theo quốc gia không?

### Phần IV: Phân tích Địa điểm (POI)
- **Q1**: Video có POI performance tốt hơn không?
- **Q2**: Thành phố nào có median share cao nhất?
- **Q3**: POI Category nào có engagement cao nhất?

### Phần V: Phân tích Nhạc (Music)
- **Q1**: Music Duration ảnh hưởng đến play_count không?
- **Q2**: Music Original vs Non-original (phân tích sâu)
- **Q3**: Top nhạc xuất hiện trong viral videos?

### Phần VI: Phân tích Thời gian
- **Q1 & Q2**: Giờ đăng ảnh hưởng đến hiệu suất như thế nào?
- **Q3**: Cuối tuần vs Ngày thường → Khi nào nên đăng?
- **Bonus**: Heatmap Giờ × Ngày → Golden window?

---

## 🛠️ Stack Công nghệ

| Thành phần | Công cụ |
|-----------|--------|
| **Dữ liệu** | Parquet files, Pandas 1.x |
| **Xử lý** | NumPy, Scikit-learn |
| **Phân tích** | SciPy, Seaborn, Matplotlib |
| **Visualize** | Seaborn, Matplotlib |
| **Mô hình** | Ridge, Random Forest, XGBoost |
| **Giải thích mô hình** | SHAP (TreeExplainer) |

---

## 📊 Kiến trúc Pipeline

```
Data Loading (Parquet) 
    ↓
Data Cleaning (null, outlier, type conversion)
    ↓
EDA (58 trực quán phân tích)
    ↓
Feature Engineering (46 features)
    ↓
Train/Test Split (80/20)
    ↓
Regression & Classification Models
    ├─ Ridge, Random Forest, XGBoost
    ├─ Model Evaluation
    ├─ Feature Importance
    └─ SHAP Explanation
```

---

## 💾 Cấu trúc Dữ liệu

**Input**: `data/*.parquet` (tất cả các file parquet)

**Cột chính**: `id`, `play_count`, `share_count`, `digg_count`, `comment_count`, `collect_count`, `duration`, `create_time`, `user_verified`, `user_tt_seller`, `is_ad`, `music_original`, `music_duration`, `poi_tt_type_name_super`, `city`, `country_code`, `desc`, `challenges`, `playlist_id`

---

## 🔧 Các Bước Xử lý Dữ liệu

### 1️⃣ **Data Cleaning** (Cells 3-14)
- Drop columns không cần (URL, avatar, stats_time)
- Parse dates (unix timestamp → datetime)
- Convert booleans ('true'/'false' → 1/0)
- Fill missing values & remove duplicates
- Winsorize outliers (1%-99% quantile)

**Kết quả**: 1M+ videos → ~950K valid videos

### 2️⃣ **EDA (58 Visualizations)** — Cells 15-54
Phân tích phân bố, tương quan, trend, category ranking

### 3️⃣ **Feature Engineering** (Cells 55-63)
46 features từ: video, music, user, time, caption, location, engagement

---

## 🤖 Kết quả Mô hình

### 📈 Regression (Predict log play_count)
| Model | R² | RMSE | MAE |
|-------|-----|------|-----|
| XGBoost | ~0.50 | ~0.9 | ~0.7 |
| Random Forest | ~0.45 | ~1.0 | ~0.8 |
| Ridge | ~0.30 | ~1.3 | ~1.0 |

**Nhận xét**: R² ~50% → play_count phụ thuộc vào nhiều yếu tố ngoài model (content quality, luck, network effects)

### 🎯 Classification (Predict Viral)
| Model | F1 | AUC-ROC |
|-------|--------|---------|
| XGBoost | ~0.70 | ~0.80 |
| Random Forest | ~0.65 | ~0.75 |
| Logistic Regression | ~0.55 | ~0.70 |

**Nhận xét**: AUC ~0.80 → phân biệt Viral/Non-viral khá tốt; F1 ~0.70 → balanced performance

---

## 🏆 Top 10 Features Quan trọng

```
1. user_verified (Verified badge) ⭐⭐⭐
2. duration (Độ dài video)
3. hour (Giờ đăng)
4. is_ad (Quảng cáo)
5. caption_length (Độ dài caption)
6. hashtag_count (Số hashtag)
7. is_prime_time (19-22h)
8. dayofweek (Ngày trong tuần)
9. music_duration (Độ dài nhạc)
10. vq_score (Chất lượng video)
```

**Kết luận**: 
- **Verified badge** = yếu tố #1 để viral
- **Giờ đăng & Duration** = rất quan trọng
- **Early engagement** quyết định FYP push

---

## 📊 Key Insights & Recommendations

### 🟰 Insights Định lượng

| Insight | Dữ liệu | Hành động |
|---------|---------|---------|
| Verified cao hơn 2-3x | Verified: 10K+ vs Unverified: 3K | Build brand → xin xác minh |
| Video 15-60s tối ưu | Peak median play ở bin này | Target short-form |
| Giờ 19-22h (prime time) | Peak median play & engagement | Đăng 8-11h hoặc 19-22h |
| Sáng Thứ 3-5 hiệu quả | Median play cao, ít cạnh tranh | Golden window |
| Nhạc trending +15-20% | Non-original > Original | Dùng trending sound + twist |
| Playlist +30-40% reach | Network effect | Tạo series |
| POI cao dân | Travel/Food POI | Gắn POI nổi tiếng |
| Hashtag #3-5 tối ưu | Phân tán reach | Tránh spam (>10) |
| Duet/Stitch +50% reach | Network cascades | Enable features |

### 💡 Creator Recommendations

1. **Build Verified Badge** (6-12 tháng)
   - Đăng consistent 3-5x/tuần
   - Reach 100K+ followers

2. **Content Strategy**
   - Duration: 15-60 giây
   - Caption: 50-100 ký tự + 3-5 hashtag
   - Add emoji (tăng engagement ~10-15%)
   - Dùng trending music (early) → unique music (after verified)

3. **Posting Schedule**
   - Weekday (Thứ 2-5) lúc 8-11h hoặc 19-22h
   - Avoid weekend (saturation cao)
   - First 2 hours = critical

4. **Maximize Early Engagement**
   - Pin comments
   - Engage with audience
   - Enable Duet/Stitch
   - Share to Story

5. **Location & POI**
   - Gắn POI nổi tiếng
   - Hashtag city name
   - Video ở thành phố lớn

---

## 🚀 Cách chạy Notebook

### Prerequisites
```bash
pip install pandas numpy scikit-learn xgboost shap matplotlib seaborn scipy
```

### Run Steps
1. **Cells 1-2**: Load libraries & data
2. **Cells 3-14**: Data cleaning
3. **Cells 15-54**: EDA (5-10 min)
4. **Cells 55-63**: Feature engineering
5. **Cells 64-72**: Train models (10-20 min)
6. **Cells 73-76**: SHAP analysis

### Output Files
```
q1_play_distribution.png
q2_duration.png
q3_verified_ad.png
... (more EDA plots)
feature_importance.png
shap_summary.png
shap_waterfall.png
```

---

## 📈 SHAP Interpretation

**SHAP values** giải thích feature nào push prediction lên (red) hoặc xuống (blue)

**Example**:
```
Video Viral:
  ✅ verified = 1      → +0.45 (push to Viral)
  ✅ hour = 20         → +0.35 (prime time)
  ✅ caption = 85      → +0.20 (good length)
  ❌ is_ad = 1         → -0.25 (pull down)
  ❌ duration = 10     → -0.15 (too short)
  ═════════════════════════════════════
  Final probability ≈ 0.75 (Viral)
```

---

## 🔍 Limitations & Future Work

### ⚠️ Limitations
1. R² ~50% → content quality + luck không capture
2. Dataset bias → chỉ TikTok, missing platform factors
3. Time-sensitive → viral trends thay đổi
4. Survivorship bias
5. Categorical explosion (>150 countries)

### 🎯 Improvements
1. **NLP features**: sentiment, keywords, language diversity
2. **Vision features**: colors, faces, scene types
3. **Temporal**: ARIMA/Prophet, season adjustment
4. **Ensemble**: Stacking, Neural Networks (LSTM)
5. **A/B testing**: validate on real creators

---

## 📚 References

- [TikTok Creator](https://tiktok.com/creator)
- [SHAP GitHub](https://github.com/slundberg/shap)
- [XGBoost Docs](https://xgboost.readthedocs.io/)
- Gladwell, M. (2000). The Tipping Point

---

## 👨‍💻 Author

**Data Analyst Project** — Phân tích Influencer & Content Strategy

---

## 📄 License

MIT License - Tự do sử dụng, modify, share với attribution

---

## 📞 Support

**Issues**: Report bugs/suggestions  
**Questions**: Data interpretation

---

## 📌 Version History

- **v1.0** (2024-2025): Complete EDA + ML + SHAP
  - 58 EDA visualizations
  - Regression + Classification models
  - SHAP feature importance
  - Actionable recommendations

---

**Last Updated**: March 2025  
**Sample Size**: ~1M TikTok videos → ~950K after cleaning
