# 🗻 Landslide Risk Mapping & ML Pipeline Summary 🧮

This guide summarizes a workflow for landslide risk mapping using Google Earth Engine (GEE), feature extraction, and machine learning (ML) models (XGBoost and CNN). It also includes visualization for model evaluation and spatial risk maps.

---

## 1️⃣ **Environment Setup**

**Packages Used:**
- `earthengine-api (ee)`: GEE access 🌐
- `geemap`: GEE Python visualization/export 🗺️
- `numpy`, `pandas`: Data manipulation 🔢
- `matplotlib`, `seaborn`: Visualization 📊
- `scikit-learn`: ML utilities 🧑‍💻
- `xgboost`: Gradient boosting ML 🌲
- `tensorflow/keras`: Deep learning 🧠

**EE Authentication:**
```python
ee.Authenticate()  # 🔑
ee.Initialize(project='gee-trial2')  # 🏁
```

---

## 2️⃣ **Area of Interest (AOI) & Timeframe** 📅

Coordinates define AOI (polygon over Maharashtra, India).  
Timeframe: **2023-01-01** to **2023-12-31**.

---

## 3️⃣ **Feature Extraction from GEE** 🛰️

Four main features are extracted and normalized:

| 🏷️ Feature         | 🌍 Source                                           | 🧮 Normalization           |
|--------------------|----------------------------------------------------|---------------------------|
| Slope              | USGS/SRTMGL1_003                                   | `/60`                     |
| NDVI               | COPERNICUS/S2_SR_HARMONIZED                        | `(-NDVI)+1`               |
| Soil Moisture      | COPERNICUS/S1_GRD (VH, VV bands)                    | `((VH-VV)/(VH+VV)+1)/2`   |
| Precipitation      | UCSB-CHG/CHIRPS/DAILY (sum)                         | `/3000`                   |

All features are stacked; 5000 sample points are taken for ML.

---

## 4️⃣ **Label Preparation** 🏷️

For demonstration, a synthetic landslide risk score is generated:
> `y = 0.4*Slope + 0.2*NDVI + 0.2*SoilMoisture + 0.2*Precipitation` ⚗️

---

## 5️⃣ **Model Training** 📈

### 🌲 **XGBoost Regression**
- 100 trees, max depth 4, learning rate 0.1
- RMSE printed for test set

### 🧠 **CNN Model**
- Sequential model: Conv2D → Flatten → Dense layers
- Inputs reshaped for CNN
- 50 epochs, batch size 32
- RMSE printed for test set

---

## 6️⃣ **Landslide Risk Map Export** 🗺️

A weighted risk map formula is applied using normalized layers:
```python
landslide_risk = norm_slope*0.4 + norm_ndvi*0.2 + norm_soil*0.2 + norm_precip*0.2  # 🏷️
geemap.ee_export_image(landslide_risk, filename='landslide_risk.tif', scale=30, region=AOI)  # 📤
```

---

## 7️⃣ **Model Evaluation Visualizations** 🎨

- **Predicted vs Actual Scatter Plots** (XGBoost & CNN) 🔵🟢
- **Residual Distribution** (Histograms with KDE) 📉
- **Histogram of Predictions** (XGBoost & CNN) 🧾

---

## 8️⃣ **Spatial Comparison Visualization** 🗾

Function `plot_compare_landslide` visualizes and compares 2D landslide risk maps from XGBoost and CNN:
- Color maps normalized to [0,1] 🎨
- Side-by-side comparison (RdYlGn_r colormap) ⬅️➡️
- Saves output PNG 🖼️

---

## 🔑 **Key Takeaways**

- **Pipeline**: GEE feature extraction → Normalization → Sampling → ML Model → Evaluation → Export/Visualization. 🔄
- **Models**: Both tree-based (XGBoost) 🌲 and neural net (CNN) 🧠; compare accuracy and spatial prediction.
- **Risk Mapping**: Exported as GeoTIFF 🗺️; visualized as heatmaps.
- **Usage**: Adapt sampling or labels for real-world risk prediction (use actual landslide events data for labels). 🌍

---

## 📚 **References**
- [Google Earth Engine](https://earthengine.google.com/) 🌐
- [COPERNICUS/S2_SR_HARMONIZED](https://developers.google.com/earth-engine/datasets/catalog/COPERNICUS_S2_SR_HARMONIZED) 🛰️
- [USGS/SRTMGL1_003](https://developers.google.com/earth-engine/datasets/catalog/USGS_SRTMGL1_003) 🌄
- [CHIRPS Precipitation](https://developers.google.com/earth-engine/datasets/catalog/UCSB-CHG_CHIRPS_DAILY) 💧
- [XGBoost Documentation](https://xgboost.readthedocs.io/en/latest/) 🌲
- [TensorFlow/Keras](https://keras.io/) 🧠

---
## ✍️ Author

Tejas Chavan  
* GIS Expert at Government Of Maharashtra Revenue & Forest Department  
* tejaskchavan22@gmail.com  
* +91 7028338510  


*This summary is based on the provided code and workflow for landslide risk mapping with GEE and ML pipelines.* ✨
