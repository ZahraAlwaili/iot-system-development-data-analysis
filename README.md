# IoT System Development and Data Analysis
 
An end-to-end IoT project: building a cloud-connected sensor system on the Arduino MKR1010, then applying time series analysis and machine learning to forecast future sensor readings.
 
**Course:** IoT Application Development
**Group:** PG-2 — Nur Afiqah, Zahra Alwaili
**Assigned sensor:** Humidity (+ mandatory built-in Temperature sensor)
 
## Overview
 
The project has two parts:
 
- **Part A** — Set up an Arduino MKR1010 with a temperature and humidity sensor, connect it to the Arduino IoT Cloud, build a monitoring dashboard, and write a Python client to pull telemetry via the Arduino Cloud API.
- **Part B** — Perform time series analysis and build a machine learning forecasting model to predict future sensor values, then report on the full methodology and findings.
## Part A: IoT System Development
 
### Task 1 — Arduino Cloud System
- Arduino MKR1010 board connected to a compatible temperature + humidity sensor
- Device registered as a "Thing" on the Arduino IoT Cloud
- Real-time monitoring dashboard with live sensor readings
- Historical data view showing at least one week of readings
**Deliverables:** board setup photo, Thing setup screenshot, exported source code, real-time dashboard screenshot, historical dashboard screenshot.
 
### Task 2 — Python Client Application
- Python application using the [Arduino IoT Cloud API](https://www.arduino.cc/reference/en/iot/api/) / [iot-client-py](https://github.com/arduino/iot-client-py) to fetch telemetry programmatically
- Retrieves sensor values with timestamps and displays them in real time
**Deliverables:** Python source code, screenshots of the app displaying timestamped sensor data.
 
## Part B: Time Series Forecasting
 
Due to data collection constraints, this project uses the **Jena Climate Dataset (2009–2016)** as the forecasting dataset (permitted as an alternative under the project brief).
 
### Objective
Predict **air temperature (T °C) 24 hours into the future** using a leakage-safe, time-aware forecasting pipeline.
 
### Methodology
- **Feature engineering**
  - Lag features at 24h, 48h, and 168h (1 week) for temperature and pressure
  - Rolling means and standard deviations over 24h windows
  - Cyclical (sine/cosine) encoding of hour, month, and wind direction to preserve circular structure
- **Validation** — `TimeSeriesSplit` with chronological train/test splitting to prevent future data leaking into training, simulating real-world deployment
- **Hyperparameter tuning** — Optuna with a Tree-structured Parzen Estimator (TPE) sampler for boosting model parameters
- **Models compared** — Ridge Regression (baseline), Random Forest, LightGBM, XGBoost
### Results
 
| Model | RMSE | MAE | R² |
|---|---|---|---|
| Ridge Regression (baseline) | 5.296 | 2.470 | 0.5613 |
| Random Forest | 3.000 | 2.356 | 0.8592 |
| **LightGBM** | **2.926** | **2.305** | **0.8661** |
| XGBoost | 2.959 | 2.323 | 0.8630 |
 
**Winning model: LightGBM** — best RMSE, MAE, and R², outperforming the linear baseline by over 33% on RMSE.
 
### Key Takeaways
- Advanced feature engineering (lags, rolling stats, cyclical encoding) and Optuna tuning were essential to capturing complex, multi-scale climate patterns
- The result is a robust, leakage-safe pipeline suitable for high-accuracy temperature forecasting in IoT applications
- **Future work:** deep learning approaches (LSTM/GRU) and incorporating additional environmental features (wind direction, humidity) could further improve accuracy
## Repository Structure
 
```
.
├── arduino-iot-dashboard/                          # Task 1: Arduino Cloud sketch & config
│   ├── G2_60307052_60306981_jun01a.ino
│   ├── thingProperties.h
│   ├── sketch.json
│   └── ReadMe.adoc
├── python-telemetry-client/                        # Task 2: Python client + collected data
│   ├── arduino_telemetry_client.ipynb
│   └── sensor_data.csv
├── code/                                           # Part B: time series analysis & forecasting model
│   └── project_partb_code.ipynb
├── IoT System Development and data Analysis.pptx   # Project presentation
├── IoT project_PA_screenshot.docx                  # Part A deliverable screenshots
├── Project_Report.docx                             # Final project report
└── README.md
```
 
> Rename `code/forecasting_model.ipynb` to match your actual notebook filename.
 
## Tech Stack
 
- **Hardware:** Arduino MKR1010, temperature & humidity sensors
- **Platform:** Arduino IoT Cloud
- **Python:** pandas, numpy, matplotlib, scikit-learn, XGBoost, LightGBM, Optuna, `arduino-iot-client`
- **Environment:** Jupyter Notebook
## Getting Started
 
1. Clone the repository
```bash
   git clone https://github.com/<your-username>/iot-system-development-data-analysis.git
```
2. Install dependencies
```bash
   pip install pandas numpy matplotlib scikit-learn xgboost lightgbm optuna arduino-iot-client
```
3. Set up your Arduino IoT Cloud credentials for the Part A Python client (see `python-telemetry-client/`)
4. Open the notebook in `code/` in Jupyter to reproduce the forecasting analysis
## Authors
 
**Nur Afiqah** & **Zahra Alwaili**
DSAI3204 — IoT Application Development, Group PG-2
 
## License
 
This project is available under the MIT License. See `LICENSE` for details.
