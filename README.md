# Analyzing the Long-Term Temperature Trends of the Varanasi Region: A Study from 1951 to 2021

**M.Sc. Thesis** · Computational Science and Applications · Banaras Hindu University (2024)
**Author:** Brijesh Kumar

---

## Overview

This project studies **71 years of daily temperature data (1951–2021)** for the Varanasi region of Uttar Pradesh, India. The aims are to:

- identify long-term warming trends and temperature anomalies,
- study seasonal and monthly patterns in temperature,
- forecast future temperatures using time-series and machine learning models.

The work covers data cleaning, exploratory data analysis (univariate, bivariate and multivariate), time-series analysis, spatial visualization, and regression modelling with Linear Regression, Decision Tree, Random Forest, K-Nearest Neighbors and SARIMA.

---

## Dataset

The data was collected from the **Department of Geophysics, Banaras Hindu University (BHU)**.

| Property | Value |
|----------|-------|
| File | `merged_data_1951_2021.csv` |
| Period | 1 Jan 1951 to 31 Dec 2021 |
| Records | 25,933 daily observations (complete daily record) |
| Location | Grid point at latitude 25.5° N, longitude 83° E (Varanasi region) |
| Missing values | 2 (in `temp`) |

**Columns**

| Column | Description |
|--------|-------------|
| `time` | Date (YYYY-MM-DD) |
| `lon` | Longitude |
| `lat` | Latitude |
| `temp` | Daily temperature (°C) |

**Summary statistics of `temp`**

| Mean | Std | Min | 25% | Median | 75% | Max |
|------|-----|-----|-----|--------|-----|-----|
| 32.02 °C | 5.66 | 13.17 °C | 28.10 °C | 32.51 °C | 35.54 °C | 45.85 °C |

---

## Methodology

```
merged_data_1951_2021.csv
        │
        ▼
1. Data Preprocessing ── drop missing values, parse dates,
        │                  extract year / month / day / season,
        │                  remove duplicates, IQR outlier removal
        ▼
preprocess 1951 to 2021.csv  (25,867 rows)
        │
        ├──► 2. Exploratory Data Analysis (univariate, bivariate, multivariate)
        ├──► 3. Time-Series & Seasonal Analysis (yearly/monthly trends, anomalies)
        ├──► 4. Spatial Visualization (Folium heatmap)
        └──► 5. Modelling (Linear Regression, Decision Tree, Random Forest, KNN, SARIMA)
                    │
                    ▼
              Evaluation: MSE, MAE, R²
```

### 1. Data Preprocessing
- Visualized missing data with `missingno` and a seaborn heatmap.
- Dropped the 2 rows with missing temperature.
- Converted `time` to datetime and extracted `year`, `month`, `day` and `season`.
- Checked for duplicate rows (none found).
- Removed outliers with the **IQR method** (1.5 × IQR), which dropped 66 records and left **25,867 rows**.
- Saved the cleaned data as `preprocess 1951 to 2021.csv`.

### 2. Exploratory Data Analysis
- **Univariate:** descriptive statistics, histograms, KDE density plots and box plots of temperature.
- **Bivariate:** temperature vs. year scatter and line plots, and correlation analysis.
- **Multivariate:** correlation heatmaps, pair plots and hex-bin joint plots.

### 3. Time-Series and Seasonal Analysis
- Yearly and monthly average temperature trends.
- Seasonal trends (Winter, Spring, Summer, Fall) and box plots of temperature by season.
- Yearly temperature anomalies relative to the long-term mean.
- **SARIMA** model to forecast average temperature for the next 10 years.

### 4. Spatial Visualization
- Interactive temperature heatmap built with **Folium**, saved as `temperature_heatmap.html`.

### 5. Machine Learning Models
Models were trained to predict temperature from the year, using an 80/20 train-test split (`random_state=42`):

- Linear Regression
- Decision Tree Regressor
- Random Forest Regressor (100 trees)
- K-Nearest Neighbors Regressor (k = 5)

---

## Results

### Model performance (cleaned data, feature = year)

| Model | MSE | MAE (°C) | R² |
|-------|-----|----------|-----|
| Linear Regression | 31.99 | 4.50 | 0.0001 |
| Decision Tree | 31.99 | 4.51 | −0.0001 |
| Random Forest | 32.00 | 4.51 | −0.0004 |

With a 5-step sequence of scaled years as input (`model.ipynb`):

| Model | MSE | MAE (°C) | R² |
|-------|-----|----------|-----|
| Decision Tree | 31.18 | 4.45 | 0.027 |
| Random Forest | 31.18 | 4.47 | 0.027 |
| KNN (k = 5) | 39.50 | 5.08 | −0.233 |

**Interpretation:** The year alone explains almost none of the day-to-day variation in temperature (R² ≈ 0), because daily temperature is dominated by the **seasonal cycle**. The correlation between year and temperature is small but positive (r ≈ 0.011), which is consistent with slow long-term warming. Better daily predictions need seasonal features such as month or day of year, or lagged temperature values.

### Key findings

- Average annual temperature in the Varanasi region rose by about **1.2 °C** over the study period.
- The frequency of extreme temperature events increased by about **10%** over the last 30 years.
- Clear seasonal patterns: the hottest temperatures fall in spring/pre-monsoon months and the coolest in winter.

---

## Repository Structure

```
.
├── merged_data_1951_2021.csv                        # Raw dataset (1951–2021)
├── preprocess 1951 to 2021.csv                      # Cleaned dataset
├── Data_Preprocessing2.ipynb                        # Missing values, date features, seasons, normalization
├── Univariate_Bivariate_Multivariate_Analysis.ipynb # EDA, IQR outlier removal, saves cleaned CSV
├── data_analysis.ipynb                              # Data types, statistics, correlations
├── ploting.ipynb                                    # Count plots, heatmaps, joint and pair plots
├── EDA.ipynb                                        # Yearly average temperature trend
├── Time_Series_analysis.ipynb                       # Yearly trends, SARIMA forecast, Folium heatmap
├── Longitude_and_Latitude_1.ipynb                   # Regional, seasonal and anomaly analysis
├── model.ipynb                                      # Linear Regression, Decision Tree, Random Forest, KNN
├── Untitled.ipynb                                   # Final models on cleaned data with plots
├── temperature_heatmap.html                         # Interactive Folium temperature map
└── README.md
```

---

## Tech Stack

- **Language:** Python 3
- **Data handling:** pandas, NumPy
- **Visualization:** Matplotlib, Seaborn, missingno, Folium
- **Machine learning:** scikit-learn
- **Time series and statistics:** statsmodels (SARIMAX), SciPy
- **Environment:** Jupyter Notebook (Anaconda)

---

## How to Run

1. Clone the repository:

   ```bash
   git clone https://github.com/brijeshpal/<repo-name>.git
   cd <repo-name>
   ```

2. Install the dependencies:

   ```bash
   pip install pandas numpy matplotlib seaborn missingno folium scikit-learn statsmodels scipy jupyter
   ```

3. Start Jupyter and run the notebooks in this order:

   1. `Data_Preprocessing2.ipynb`
   2. `Univariate_Bivariate_Multivariate_Analysis.ipynb` (creates `preprocess 1951 to 2021.csv`)
   3. `Time_Series_analysis.ipynb` and `Longitude_and_Latitude_1.ipynb`
   4. `model.ipynb` and `Untitled.ipynb`

   ```bash
   jupyter notebook
   ```

4. Open `temperature_heatmap.html` in a browser to view the interactive map.

---

## Limitations

- The dataset covers a **single grid point** (25.5° N, 83° E), so the regional comparison and ANOVA test in `Longitude_and_Latitude_1.ipynb` have only one region to work with.
- The SARIMA model uses a seasonal period of 12 on **yearly** averages; a seasonal period fits monthly data better.
- The regression models use only the year as a feature, so they cannot capture seasonal variation.
- Some notebooks depend on variables from earlier sessions (for example, `EDA.ipynb` needs the `year` column created during preprocessing), so run them in the order above.

---

## Future Work

- Add more climate variables such as humidity, precipitation, wind speed and atmospheric pressure.
- Analyze **maximum and minimum temperatures** separately to better understand extreme events.
- Add seasonal and lag features (month, day of year, previous days' temperatures) to the models.
- Try deep learning models for time series, such as **LSTM** and **GRU**.
- Extend the study to multiple grid points across the region for a true spatial analysis.

---

## Conclusion

Understanding long-term temperature trends is important for local authorities and policymakers in the Varanasi region. The findings of this study can help in planning climate action and adaptation strategies to reduce the effects of rising temperatures and more frequent extreme heat.

---

## Acknowledgements

- **Department of Geophysics, Banaras Hindu University** for providing the temperature dataset.
- My thesis supervisor *Prof. Manjari Gupta* for guidance throughout this work.

## Contact

**Brijesh Kumar**
- Email: [brijeshpal8878@gmail.com](mailto:brijeshpal8878@gmail.com)
- GitHub: [brijeshpal](https://github.com/brijeshpal)
- LinkedIn: [brijeshkumar8878](https://www.linkedin.com/in/brijeshkumar8878)
- Homepage: [brijeshpal.github.io](https://brijeshpal.github.io)
