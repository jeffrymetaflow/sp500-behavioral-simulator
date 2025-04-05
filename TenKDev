import streamlit as st
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

st.set_page_config(page_title="S&P 500 Behavioral Recovery Simulator", layout="wide")
st.title("📉 Behavioral Macro Overlay: S&P 500 Recovery After Tariff Shock")

# Sidebar inputs
st.sidebar.header("Simulation Parameters")
initial_sp500 = st.sidebar.number_input("Initial S&P 500 Index Level", value=5000, step=100)
shock_pct = st.sidebar.slider("Initial Shock Drop (%)", min_value=1, max_value=20, value=6)
k = st.sidebar.slider("Recovery Steepness (k)", min_value=0.01, max_value=0.2, value=0.07)
x0 = st.sidebar.slider("Recovery Midpoint Day (x0)", min_value=10, max_value=100, value=45)
duration = st.sidebar.slider("Simulation Duration (Days)", min_value=30, max_value=365, value=120)
noise_level = st.sidebar.slider("Sentiment Noise Level", min_value=0, max_value=50, value=15)

# Simulation logic
days = np.arange(0, duration + 1)
shock_drop = (shock_pct / 100) * initial_sp500
L = initial_sp500

# Logistic recovery model
recovery_curve = L - (shock_drop) / (1 + np.exp(-k * (days - x0)))
sentiment_noise = np.random.normal(0, noise_level, size=len(days))
sp500_simulated = recovery_curve + sentiment_noise

# Plotting
fig, ax = plt.subplots(figsize=(12, 6))
ax.plot(days, sp500_simulated, label="Simulated S&P 500", color="blue")
ax.axhline(initial_sp500, color='gray', linestyle='--', label="Pre-Tariff Level")
ax.axvline(0, color='red', linestyle=':', label="Tariff Shock Day")
ax.set_title("Simulated Recovery of the S&P 500 After Tariff Shock")
ax.set_xlabel("Days Since Tariff Shock")
ax.set_ylabel("S&P 500 Index Level")
ax.legend()
ax.grid(True)

# Show chart
st.pyplot(fig)

# Show data if toggled
if st.checkbox("Show raw data"):
    df = pd.DataFrame({"Day": days, "S&P 500 Index (Simulated)": sp500_simulated})
    st.dataframe(df.round(2))
