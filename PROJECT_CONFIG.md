# Project Configuration & Notes

## Dataset Information

**Source**: TikTok video analytics data  
**Format**: Parquet files  
**Total Records**: ~1M+ videos  
**After cleaning**: ~950K valid videos  
**Date Range**: [Từ dataset]  

## Data Columns (Key)

### Engagement Metrics
- `play_count` - Lượt xem
- `share_count` - Lượt chia sẻ
- `digg_count` - Lượt thích (like)
- `comment_count` - Số bình luận
- `collect_count` - Lượt lưu

### Video Properties
- `duration` - Độ dài video (giây)
- `vq_score` - Video quality score
- `create_time` - Thời gian tạo
- `desc` - Caption/mô tả

### User Properties
- `user_verified` - Có xác minh
- `user_tt_seller` - TikTok seller
- `user_id` - User ID

### Music & Content
- `music_original` - Nhạc gốc (0/1)
- `music_duration` - Độ dài nhạc
- `music_title` - Tên bài nhạc
- `challenges` - Challenge list

### Features & Location
- `duet_enabled` - Cho phép Duet
- `stitch_enabled` - Cho phép Stitch
- `poi_tt_type_name_super` - Loại địa điểm
- `city` - Thành phố
- `country_code` - Mã quốc gia
- `playlist_id` - Playlist ID

### Meta
- `id` - Video ID (unique)
- `is_ad` - Là quảng cáo
- `collected_time` - Thời gian thu thập

---

## Processing Pipeline

### 1. Data Loading
- Load tất cả `.parquet` files từ `data/`
- Concatenate thành single DataFrame

### 2. Data Cleaning
- **Step 1**: Drop unnecessary columns (URL, avatar links, etc)
- **Step 2**: Parse datetime columns
- **Step 3**: Convert boolean columns
- **Step 4**: Fill missing values strategically
- **Step 5**: Remove duplicates by `id`
- **Step 6**: Filter invalid records (play_count=0, duration=0)
- **Step 7**: Winsorize outliers (1%-99% bounds)

**Output**: Clean, processed dataset

### 3. EDA Phase
- **Part I** (Cells 15-26): General analysis & distributions
- **Part II** (Cells 27-50): Content & features impact
- **Part III** (Cells 51-58): User & geographic analysis
- **Part IV** (Cells 59-66): Time-based analysis

**Output**: 58 visualizations to `outputs/`

### 4. Feature Engineering
- Create 46 engineered features
- Target: `is_viral` (top 15%), `play_count` (log)

### 5. Modeling
- **Regression**: Predict log(play_count)
  - Ridge, Random Forest, XGBoost
- **Classification**: Predict Viral (1) vs Non-viral (0)
  - Logistic Regression, Random Forest, XGBoost

### 6. Interpretation
- Feature importance rankings
- SHAP values analysis
- Waterfall plots for individual predictions

---

## Key Metrics & Thresholds

| Metric | Definition | Threshold |
|--------|-----------|-----------|
| `is_viral` | Top 15% by Viral Score | `viral_score >= quantile(0.85)` |
| `viral_score` | Weighted engagement | 0.35×play + 0.30×share + 0.20×like + 0.10×comment + 0.05×collect |
| `engagement_rate` | Total engagement / play | (digg + comment + share) / play |
| `prime_time` | Peak viewing hours | hour in [19, 20, 21, 22] |
| `golden_hour` | Content upload window | hour in [8, 9, 10, 11] |

---

## Output Artifacts

### Visualizations (saved to `outputs/`)

**EDA Plots**:
- `q1_play_distribution.png` - Play count distribution
- `q2_duration.png` - Duration impact
- `q3_verified_ad.png` - Verified & Ad impact
- `q4_correlation.png` - Correlation matrix
- `q5_duration_linear.png` - Duration linearity
- `q_music_*.png` - Music analysis (3 plots)
- `q_duet_stitch.png` - Duet/stitch impact
- `q_poi_*.png` - POI analysis (2 plots)
- `q_city.png` - City-level analysis
- `q_country.png` - Country-level analysis
- `q_hourly.png` - Hourly analysis
- `q_daily.png` - Daily analysis
- `q_heatmap_hour_day.png` - Hour×Day heatmap
- `q_playlist.png` - Playlist impact
- `q_seller_verified.png` - Seller & verified

**Model Plots**:
- `feature_importance.png` - Top features ranking
- `shap_summary.png` - SHAP mean absolute impact
- `shap_waterfall.png` - Individual prediction explanation

### Console Output
- Model performance metrics (R², AUC, F1)
- Feature importance values
- Classification reports with precision/recall/f1

---

## Hyperparameters (Current)

### XGBoost Regression
```python
XGBRegressor(
    n_estimators=200,
    max_depth=6,
    learning_rate=0.1,
    subsample=0.8,
    colsample_bytree=0.8,
    min_child_weight=10,
    random_state=42
)
```

### XGBoost Classification
```python
XGBClassifier(
    n_estimators=200,
    max_depth=6,
    learning_rate=0.1,
    subsample=0.8,
    colsample_bytree=0.8,
    min_child_weight=10,
    scale_pos_weight=(neg/pos),
    eval_metric='logloss',
    random_state=42
)
```

### Other Models
- Ridge: `alpha=1.0`
- Random Forest: `n_estimators=100, max_depth=10, min_samples_leaf=20`
- Logistic: `max_iter=1000, C=1.0`

---

## Performance Baselines

| Task | Model | Score | Notes |
|------|-------|-------|-------|
| Regression | XGBoost | R²=0.50 | 50% variance explained |
| Regression | Random Forest | R²=0.45 | Slightly lower |
| Regression | Ridge | R²=0.30 | Linear limitation |
| Classification | XGBoost | AUC=0.80 | Good discrimination |
| Classification | RF | AUC=0.75 | Lower |
| Classification | Logistic | AUC=0.70 | Baseline |

---

## Next Steps / TODO

- [ ] Validate on unseen test set
- [ ] Hyperparameter tuning (GridSearchCV)
- [ ] Add NLP features (sentiment, keywords)
- [ ] Add vision features (colors, faces)
- [ ] Implement LSTM for temporal patterns
- [ ] A/B test recommendations on real creators
- [ ] Create production API endpoint
- [ ] Real-time prediction dashboard

---

## References & Resources

- **SHAP**: https://github.com/slundberg/shap
- **XGBoost**: https://xgboost.readthedocs.io/
- **Scikit-learn**: https://scikit-learn.org/
- **Pandas**: https://pandas.pydata.org/
- **TikTok Algorithm**: https://newsroom.tiktok.com/

---

**Last Updated**: March 2025  
**Maintainer**: Data Analyst Team
