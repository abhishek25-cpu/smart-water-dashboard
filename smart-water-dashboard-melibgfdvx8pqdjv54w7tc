# streamlit_app.py
import streamlit as st
import pandas as pd
import numpy as np
import altair as alt

# Sample data generation
np.random.seed(42)
dates = pd.date_range("2024-01-01", "2024-01-30")
processes = ["Cooling", "Cleaning", "Dyeing"]

data = []
for date in dates:
    for process in processes:
        base = {"Cooling": 5000, "Cleaning": 3000, "Dyeing": 4000}[process]
        usage = np.random.normal(loc=base, scale=200)
        data.append({"Date": date, "Process": process, "Water_Usage_Liters": usage})

df = pd.DataFrame(data)
df["Flow_Rate_LPH"] = df["Water_Usage_Liters"] / 24
df["Anomaly"] = False

# Add 5 random anomalies
anomalies_idx = np.random.choice(df.index, 5, replace=False)
df.loc[anomalies_idx, "Water_Usage_Liters"] *= np.random.choice([0.5, 1.5, 2], 5)
df.loc[anomalies_idx, "Anomaly"] = True

# --- Dashboard ---
st.title("💧 Smart Water Usage Dashboard")
st.markdown("A live dashboard to monitor water consumption and detect anomalies in manufacturing processes.")

# Daily water usage
st.header("📈 Daily Water Usage by Process")
daily_usage = df.groupby(["Date", "Process"])["Water_Usage_Liters"].sum().reset_index()

chart = alt.Chart(daily_usage).mark_line().encode(
    x='Date:T',
    y='Water_Usage_Liters:Q',
    color='Process:N'
).properties(width=700)

st.altair_chart(chart, use_container_width=True)

# Anomaly highlight
st.header("🚨 Anomalies Detected")
anomaly_data = df[df["Anomaly"] == True]
st.dataframe(anomaly_data.style.highlight_max(axis=0, color='tomato'))

# Flow Rate vs Benchmark
st.header("📍 Flow Rate vs Benchmark")
benchmark = 200  # LPH
df["Benchmark_Diff"] = df["Flow_Rate_LPH"] - benchmark
st.line_chart(df.groupby("Date")["Flow_Rate_LPH"].mean())

# Water usage by process (Pie)
st.header("📊 Total Water Use by Process")
total_usage = df.groupby("Process")["Water_Usage_Liters"].sum()
st.bar_chart(total_usage)

st.markdown("---")
st.caption("Built with ❤️ using Streamlit. Designed for mobile monitoring.")

