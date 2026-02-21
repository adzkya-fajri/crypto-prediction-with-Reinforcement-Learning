# Reinforcement Learning for Financial Trading

## Overview

Project ini mengimplementasikan **Reinforcement Learning (RL)** untuk strategi trading menggunakan algoritma **Proximal Policy Optimization (PPO)**.

Model belajar menentukan posisi:

* `0 = Short`
* `1 = Long`

berdasarkan data historis log return harga.

Tujuan utama:

> Mengoptimalkan profit melalui pembelajaran berbasis reward.

---

# Konsep Dasar

Berbeda dengan supervised learning, RL tidak menggunakan target label langsung.

RL bekerja dengan skema:

```
State → Action → Reward → Next State
```

Di project ini:

| Komponen  | Implementasi                           |
| --------- | -------------------------------------- |
| State     | 10 hari terakhir log_return            |
| Action    | Short / Long                           |
| Reward    | Posisi × log_return − transaction_cost |
| Objective | Maksimalkan cumulative reward          |

---

# Dataset

Data yang digunakan:

* Close price
* Volume

Fitur yang dihitung:

```python
df['log_return'] = np.log(df['Close'] / df['Close'].shift(1))
```

Target dalam RL bukan label eksplisit, tetapi reward dari environment.

---

# Environment Design

Custom environment dibuat menggunakan `gymnasium`.

###  Observation Space

Window 10 hari log_return:

```
[ r(t-9), r(t-8), ..., r(t) ]
```

###  Action Space

```
0 → Short
1 → Long
```

###  Reward Function

```
reward = position × log_return - transaction_cost
```

Jika agent mengganti posisi, dikenakan biaya transaksi:

```
transaction_cost = 0.0005
```

---

# Model

Model yang digunakan:

```
PPO (Proximal Policy Optimization)
```

Konfigurasi utama:

```python
learning_rate = 0.0003
n_steps = 64
batch_size = 32
total_timesteps = 20000
```

---

# Evaluasi

Evaluasi dilakukan dengan menghitung:

* Total reward
* Final balance

Interpretasi:

```
Final Balance > 1 → Profit
Final Balance < 1 → Loss
```

---

# Important Notes

Dataset hanya ±256 data harian.

Risiko:

* Overfitting tinggi
* RL tidak stabil dengan data kecil
* Reward sangat sensitif

Rekomendasi:

* Gunakan minimal 2000+ data
* Tambahkan indikator teknikal
* Gunakan train/test split time-series
* Normalisasi observation

---

# Future Improvements

Beberapa pengembangan lanjutan:

* Tambahkan indikator (RSI, Moving Average, Volatility)
* Gunakan Sharpe Ratio sebagai reward
* Tambahkan position sizing
* Gunakan vectorized environment
* Tambahkan early stopping
* Backtesting lebih realistis

---

# Dependencies

```
numpy
pandas
matplotlib
gymnasium
stable-baselines3
```

Install:

```
pip install numpy pandas matplotlib gymnasium stable-baselines3
```

---

#  Perbandingan Pendekatan

| Supervised Learning     | Reinforcement Learning  |
| ----------------------- | ----------------------- |
| Prediksi harga          | Belajar strategi        |
| Butuh target            | Butuh reward            |
| Lebih stabil data kecil | Butuh banyak data       |
| Mudah evaluasi          | Evaluasi lebih kompleks |

---

#  Conclusion

Reinforcement Learning memungkinkan model:

* Belajar strategi trading secara dinamis
* Mengoptimalkan profit langsung
* Beradaptasi terhadap perubahan market

Namun:

> RL membutuhkan data besar dan environment yang dirancang dengan baik agar stabil."# crypto-prediction-with-Reinforcement-Learning" 
