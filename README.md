import streamlit as st
import pandas as pd
import numpy as np
import plotly.express as px

st.set_page_config(page_title="Road Energy Ghana", page_icon="⚡", layout="wide")

st.title("⚡ Road Energy Potential in Ghana")
st.caption("Kinetic energy from vehicles on the road → homes powered. Built at 17, on a phone, in SHS. 🇬🇭")

# ----------------------------- DATA -----------------------------
data = {
    'Region': ['Greater Accra','Ashanti','Western','Central','Eastern','Northern','Volta',
               'Bono','Upper East','Upper West','Ahafo','Bono East','Oti','Savannah',
               'North East','Western North'],
    'Vehicle_Count': [1200000,750000,320000,280000,260000,180000,150000,140000,90000,
                      70000,85000,95000,65000,55000,50000,60000],
    'Avg_Speed_kmh': [35,45,55,50,60,65,60,60,65,65,55,55,60,65,65,55],
    'Avg_Mass_kg': [1800,1700,1900,1750,1800,2000,1850,1800,2100,2100,1750,1800,1900,
                    2000,2050,1850],
    'Household_kWh_Month': [300,280,250,240,230,200,210,220,190,185,220,225,210,195,190,235],
    'Road_Quality_Factor': [0.85,0.80,0.75,0.72,0.70,0.65,0.68,0.67,0.62,0.60,0.66,0.67,
                            0.64,0.63,0.62,0.70],
}
df = pd.DataFrame(data)
df['Cars'] = (df['Vehicle_Count'] * 0.55).astype(int)
df['Buses'] = (df['Vehicle_Count'] * 0.15).astype(int)
df['Trucks'] = (df['Vehicle_Count'] * 0.10).astype(int)
df['Motorcycles'] = (df['Vehicle_Count'] * 0.20).astype(int)

# ----------------------------- SIDEBAR -----------------------------
st.sidebar.header("⚙️ Model assumptions")
efficiency = st.sidebar.slider("Energy capture efficiency (%)", 5, 50, 25, 5) / 100.0
trips = st.sidebar.slider("Trips per vehicle per day", 1, 6, 2)
km_per_trip = st.sidebar.slider("Km per trip", 5, 60, 25)
st.sidebar.markdown("---")
st.sidebar.info("KE = ½·m·v², adjusted by road quality. Move the sliders to stress-test the model.")

# ----------------------------- MODEL -----------------------------
DAYS = 30
J_PER_KWH = 3_600_000
df['Speed_ms'] = (df['Avg_Speed_kmh'] / 3.6) * df['Road_Quality_Factor']
df['KE_Joules'] = 0.5 * df['Avg_Mass_kg'] * (df['Speed_ms'] ** 2)
df['Daily_Energy_J'] = df['KE_Joules'] * df['Vehicle_Count'] * trips * km_per_trip * efficiency
df['Monthly_kWh'] = (df['Daily_Energy_J'] * DAYS) / J_PER_KWH
df['Homes_Powered'] = (df['Monthly_kWh'] / df['Household_kWh_Month']).astype(int)

total_kwh = df['Monthly_kWh'].sum()
total_homes = int(df['Homes_Powered'].sum())
total_vehicles = int(df['Vehicle_Count'].sum())
pct_households = total_homes / 6_000_000 * 100

# ----------------------------- HEADLINE METRICS -----------------------------
c1, c2, c3, c4 = st.columns(4)
c1.metric("🚗 Vehicles modelled", f"{total_vehicles:,}")
c2.metric("⚡ Monthly kWh", f"{total_kwh:,.0f}")
c3.metric("🏠 Homes powered", f"{total_homes:,}")
c4.metric("🇬🇭 % of households", f"{pct_households:.2f}%")

# ----------------------------- CHARTS -----------------------------
tab1, tab2, tab3 = st.tabs(["🏠 Homes powered", "⚡ Energy by region", "🚗 Fleet mix"])

with tab1:
    d = df.sort_values('Homes_Powered', ascending=True)
    fig = px.bar(d, x='Homes_Powered', y='Region', orientation='h', color='Homes_Powered',
                 color_continuous_scale='Reds', text='Homes_Powered')
    fig.update_traces(texttemplate='%{text:,}', textposition='outside')
    fig.update_layout(yaxis_title='', xaxis_title='Homes powered per month',
                      height=600, showlegend=False)
    st.plotly_chart(fig, use_container_width=True)

with tab2:
    d = df.sort_values('Monthly_kWh', ascending=True)
    fig = px.bar(d, x='Monthly_kWh', y='Region', orientation='h', color='Monthly_kWh',
                 color_continuous_scale='Greens', text=df['Monthly_kWh'].apply(lambda v: f"{v/1e6:.2f}M"))
    fig.update_traces(textposition='outside')
    fig.update_layout(yaxis_title='', xaxis_title='Monthly energy potential (kWh)',
                      height=600, showlegend=False)
    st.plotly_chart(fig, use_container_width=True)

with tab3:
    fleet = {'Cars': df['Cars'].sum(), 'Buses': df['Buses'].sum(),
             'Trucks': df['Trucks'].sum(), 'Motorcycles': df['Motorcycles'].sum()}
    fig = px.pie(names=list(fleet.keys()), values=list(fleet.values()), hole=0.4,
                 color_discrete_sequence=['#3498db', '#e74c3c', '#f39c12', '#2ecc71'])
    fig.update_traces(textinfo='percent+label')
    st.plotly_chart(fig, use_container_width=True)

# ----------------------------- TABLE -----------------------------
with st.expander("📋 Full regional breakdown"):
    show = df[['Region','Vehicle_Count','Avg_Speed_kmh','Monthly_kWh','Homes_Powered']].copy()
    show['Monthly_kWh'] = show['Monthly_kWh'].apply(lambda v: f"{v:,.0f}")
    show['Vehicle_Count'] = show['Vehicle_Count'].apply(lambda v: f"{v:,}")
    show['Homes_Powered'] = show['Homes_Powered'].apply(lambda v: f"{v:,}")
    st.dataframe(show.sort_values('Homes_Powered', ascending=False, key=lambda s: s.str.replace(',','').astype(int)),
                 use_container_width=True, hide_index=True)

# ----------------------------- HONESTY -----------------------------
with st.expander("⚠️ Assumptions & limitations (please read)"):
    st.markdown("""
This is a **first-order estimation model**, not a feasibility study.
- **Synthetic dataset** anchored to DVLA / GSS / GHA published figures (e.g. the 2010
  national road-condition mix 42% good / 28% fair / 30% poor is reflected in the
  road-quality factors).
- We treat kinetic energy as harvestable per distance travelled; a stricter version
  models **discrete braking events** (energy captured each time a vehicle slows).
- Real piezoelectric / regenerative capture rates vary; the slider lets you explore them.
The point: the **order of magnitude is real** and worth studying for road-energy policy.
""")

st.markdown("---")
st.caption("Built by Collins · Python · Pandas · Plotly · Streamlit · Google Colab")
