# ⚡ Road Energy Potential in Ghana

[![Streamlit App](https://static.streamlit.io/badges/streamlit_badge_black_white.svg)](https://road-energy-ghana-u2da6zh7v2lrgtdtzrwgle.streamlit.app/))

> *What if the cars stuck in Accra traffic were quietly generating electricity?*

This project models how much **kinetic energy** could be harvested from vehicles
moving on Ghana's roads (regenerative braking / piezoelectric surfaces) and
translates it into **homes powered**, region by region — with both a static
notebook and a **live interactive app**.

🧑🏾‍💻 Built by **Collins**, 17 — a SHS sophomore in Ghana — on a phone.

## 🎮 Try the live app
👉 **[Open the interactive dashboard]([*](https://road-energy-ghana-u2da6zh7v2lrgtdtzrwgle.streamlit.app/)* — drag the sliders
(efficiency, trips/day, km/trip) and watch the numbers change.

## ❓ The question
Ghana loses productive hours to *dumsor*. Meanwhile millions of vehicles carry
enormous kinetic energy, wasted as brake heat. **Could we capture a fraction of
it — and how many homes would it light up?**

## 🧠 Approach
1. 16-region vehicle dataset (cars, buses, trucks, motorcycles) anchored to DVLA & GSS.
2. Kinetic energy per vehicle: **KE = ½·m·v²**, adjusted by a regional road-quality factor.
3. **25%** energy-capture efficiency (slider-adjustable in the app).
4. Joules → **kWh/month** → divide by regional household use (ECG-based) → **homes powered**.

## 📊 Key results (at 25% efficiency)
| Metric | Value |
|---|---|
| 🚗 Vehicles modelled | 3,850,000 |
| ⚡ Monthly energy potential | 37,082,838 kWh |
| 🏠 Homes powered | 152,899 |
| 🇬 Share of ~6M households | ≈ 2.55% |

| Region | Vehicles | Monthly kWh | Homes |
|---|---:|---:|---:|
| Greater Accra | 1,200,000 | 7,682,825 | 25,609 |
| Ashanti | 750,000 | 6,640,625 | 23,716 |
| Western | 320,000 | 4,157,633 | 16,630 |
| Eastern | 260,000 | 3,317,708 | 14,424 |
| Northern | 180,000 | 2,582,556 | 12,912 |
| Central | 280,000 | 2,552,083 | 10,633 |
| Volta | 150,000 | 1,856,424 | 8,840 |
| Bono | 140,000 | 1,636,632 | 7,439 |
| Upper East | 90,000 | 1,233,575 | 6,492 |
| Upper West | 70,000 | 898,548 | 4,857 |
| Bono East | 95,000 | 933,181 | 4,147 |
| Savannah | 55,000 | 741,301 | 3,801 |
| Ahafo | 85,000 | 787,707 | 3,580 |
| North East | 50,000 | 668,999 | 3,521 |
| Oti | 65,000 | 731,852 | 3,485 |
| Western North | 60,000 | 661,209 | 2,813 |

## 📁 Repo structure
- `road_energy_ghana.ipynb` — the full analysis + 4 charts (renders on GitHub)
- `app.py` — the interactive Streamlit dashboard
- `requirements.txt` — Python dependencies
- `README.md` — this file

## ▶️ Run locally
```bash
# Notebook: open road_energy_ghana.ipynb in Google Colab -> Run all
# App:
pip install -r requirements.txt
streamlit run app.py
