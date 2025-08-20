# CNN-LSTM_for_vehicle_information_reconstruction

## 📖 Overview
This repository implements and compares three imputation methods for handling missing predecessor state information in Multi-Predecessor Following (MPF) Connected and Automated Vehicle (CAV) platoons:
- **Statistical Mean Interpolation (Baseline)**
- **CNN-LSTM Model**
- **Transformer Model**

The framework simulates vehicle platoon dynamics under different data loss rates, trains imputation models, and evaluates both imputation accuracy and control performance.

---

## 📂 File Structure
```
MPF Imputation/
│── MPFenv.py                    # Environment simulator, generates raw data
│── cnntrain.py                  # Train CNN-LSTM model
│── transformertrain.py          # Train Transformer model
│── cnn.py                       # CNN-LSTM inference under varying missing rates
│── transformer.py               # Transformer inference under varying missing rates
│── baseline.py                  # Statistical mean baseline method
│── benchmark_inference.py       # Benchmark inference latency & throughput
│── metrics calculation.py       # Calculate control metrics
│── *.csv                        # Generated imputation results
│── *.keras                      # Trained model weights
│── *.pkl                        # Scalers for input/output normalization
│── matrics.xlsx                 # Collected experimental results
```

---

## ⚙️ Installation
```bash
git clone https://github.com/LukeZhu0227/CNN-LSTM_for_vehicle_information_reconstruction
cd MPF-Imputation
pip install -r requirements.txt
```

---

## 🚀 Usage

### 1. Generate Simulation Data
```bash
python MPFenv.py
```
This creates `mpf_simulation_output.csv`.

---

### 2. Train Models
- **CNN-LSTM**
```bash
python cnntrain.py
```
Generates:
- `cnn_scaler.pkl`  
- `cnn_label_scaler.pkl`  
- `cnn_model_with_mask.keras`  

- **Transformer**
```bash
python transformertrain.py
```
Generates:
- `transformer_scaler.pkl`  
- `transformer_label_scaler.pkl`  
- `transformer_model_with_mask.keras`

---

### 3. Run Imputation
- **CNN-LSTM**  
Edit line **15** in `cnn.py`:
```python
MISSING_RATE = 0.3  # example
```
Then run:
```bash
python cnn.py
```
Outputs CSV files like `cnn_30%missing_complete.csv`. In addition, the script reports **MSE/MAE** for **velocity** and **spacing**.

- **Transformer**  
Edit line **39** in `transformer.py`:
```python
rate = 0.3
```
Then run:
```bash
python transformer.py
```
Outputs CSV files like `transformer_30%missing_complete.csv`. In addition, the script reports **MSE/MAE** for **velocity** and **spacing**.

- **Baseline (Statistical Mean)**  
Edit line **18** in `baseline.py`:
```python
rate = 0.3
```
Then run:
```bash
python baseline.py
```
Outputs CSV files like `baseline_30%missing_complete.csv`. In addition, the script reports **MSE/MAE** for **velocity** and **spacing**.

---

### 4. Benchmark Inference Efficiency
```bash
python benchmark_inference.py
```
Reports latency (ms/sample) and throughput (samples/s) for all methods.

---

### 5. Calculate Metrics
Edit line **5** in `metrics calculation.py` to specify the path of result files, e.g.:
```python
file_path = "cnn_30%missing_complete.csv"
```
Then run:
```bash
python "metrics calculation.py"
```
Outputs control-oriented metrics (e.g., Time-to-Collision, velocity variance, average speed).

---

## 📊 Results
- CNN-LSTM achieves the lowest imputation error and best control performance under most missing rates.
- Transformer maintains moderate accuracy with higher computational cost.
- Baseline method degrades significantly as missing rate increases.

---

## 📑 Citation
If you use this repository, please cite our paper:
> [Learning-Based Imputation Model for Connected and Automated Vehicle Platoons under Partial Observability]  
> Haoheng Zhu, 2025  
