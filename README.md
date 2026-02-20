# smart-drug-supply-chain
Smart Drug Supply Chain and Shortage Prediction Platform
import streamlit as st

st.set_page_config(page_title="Smart Drug Supply Chain", layout="wide")

st.title("💊 Smart Drug Supply Chain & Shortage Prediction Platform")
st.write("Predict medicine shortages before they happen")

surge = st.slider("Disease / Seasonal Surge Factor", 1.0, 2.5, 1.0)

hospitals = [
    {"hospital": "Hospital A", "stock": 500, "demand": 100, "delay": 5},
    {"hospital": "Hospital B", "stock": 1200, "demand": 80, "delay": 5},
    {"hospital": "Hospital C", "stock": 300, "demand": 120, "delay": 5},
]

def predict(stock, demand, delay, surge):
    demand = demand * surge
    days_left = stock / demand

    if days_left < delay:
        return days_left, "HIGH"
    elif days_left <= delay + 2:
        return days_left, "MEDIUM"
    else:
        return days_left, "LOW"

st.subheader("📊 Hospital Risk Analysis")

for h in hospitals:
    days, risk = predict(h["stock"], h["demand"], h["delay"], surge)

    st.write(f"### {h['hospital']}")
    st.write(f"Days remaining: {days:.2f}")

    if risk == "HIGH":
        st.error("🔴 High Risk – Shortage likely")
    elif risk == "MEDIUM":
        st.warning("🟡 Medium Risk – Monitor closely")
    else:
        st.success("🟢 Low Risk – Stock sufficient")
