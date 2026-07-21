# ⚡ Road Energy Potential in Ghana

> *What if the cars stuck in Accra traffic were quietly generating electricity?*

This project models how much **kinetic energy** could theoretically be harvested
from vehicles moving on Ghana's roads (e.g. via regenerative braking or
piezoelectric road surfaces) — and translates that into **the number of homes it
could power**, region by region.

🧑🏾‍💻 Built by **Collins**, age 17 — a SHS sophomore in Ghana — using only a phone.
🐍 Python · Pandas · NumPy · Matplotlib · Google Colab

---

## ❓ The Question
Ghana loses a lot of productive hours (and patience) to *dumsor* — intermittent
power supply. Meanwhile, millions of vehicles move across the country every day,
carrying enormous kinetic energy that is normally wasted as heat in the brakes.

**Could we capture even a fraction of that energy — and how many homes would it light up?**

## 🧠 The Approach
1. Build a 16-region dataset of registered vehicles (cars, buses, trucks,
   motorcycles), grounded in DVLA & Ghana Statistical Service estimates.
2. Compute kinetic energy per vehicle: **KE = ½ · m · v²**, adjusted by a
   regional road-quality factor.
3. Apply a **25% energy-capture efficiency** (a realistic figure for
   regenerative / harvesting systems).
4. Convert joules → **kWh per month**.
5. Divide by average household consumption (ECG-based, ~185–300 kWh/month by
   region) → **homes powered**.

## 📊 Key Results

| Metric | Value |
|---|---|
| 🚗 Vehicles modelled | **3,850,000** |
| ⚡ Monthly energy potential | **37,082,838 kWh** |
| 🏠 Homes that could be powered | **152,899** |
| 🇬🇭 Share of Ghana's ~6M households | **≈ 2.55%** |

### By region (sorted by homes powered)

| Region | Vehicles | Monthly kWh | Homes Powered |
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

> 🔎 **Note:** Ashanti powers nearly as many homes as Greater Accra despite
> fewer vehicles — because higher average speeds and lower per-household
> consumption amplify the impact per vehicle. Trucks punch above their weight
> too: kinetic energy scales with **mass**, so the heavy commercial fleet
> contributes disproportionately.

## 📈 Visualizations
Four charts are generated and embedded in the notebook (they render directly
when you open `road_energy_ghana.ipynb` on GitHub):
1. Monthly energy potential by region
2. Homes powered by region
3. Vehicle fleet composition
4. Road quality vs energy potential (bubble chart)

## ⚠️ Assumptions & Limitations (please read)
Transparency matters more than a big number. This is a **first-order estimation
model**, not a feasibility study:
- **Synthetic dataset.** Vehicle counts and speeds are realistic estimates
  anchored to published DVLA / GSS / GHA figures (e.g. the 2010 national
  road-condition mix of 42% good / 28% fair / 30% poor is reflected in the
  regional road-quality factors). Real per-region registration data would
  sharpen it.
- **Energy model.** We treat kinetic energy as harvestable per distance
  travelled. A more rigorous v2 will model **discrete braking events** (energy
  captured each time a vehicle decelerates), which is the physically cleaner
  framing for regenerative harvesting.
- **Efficiency = 25%** is an optimistic-but-plausible capture rate; real-world
  piezoelectric / regen systems vary widely.
- **Household consumption** uses regional ECG-based averages.

The point of this project is to show that the **order of magnitude is real and
worth studying** — and to make the case for energy-harvesting infrastructure in
road planning.

## 🚀 How to run
Open the notebook in Google Colab and run all cells:

    # In Google Colab: Runtime -> Run all
    # Or locally:
    pip install pandas numpy matplotlib
    jupyter notebook road_energy_ghana.ipynb

No external data files required — the dataset is generated inside the notebook.

## 🛠️ Tech Stack
Python · Pandas · NumPy · Matplotlib · Google Colab

## 🔜 Next Steps
- [ ] Refine the physics to a discrete braking-event model (v2.1)
- [ ] Sensitivity analysis across efficiency (15% / 25% / 35%)
- [ ] Cross-check with real DVLA registration CSVs
- [ ] Interactive Streamlit dashboard
- [ ] Cost-benefit vs road-construction spend (GHA spends US$100M+/yr on roads)

## 🧑🏾‍💻 About
I'm Collins, 17, a senior-high-school sophomore in Ghana teaching myself data
science, machine learning and AI (Kaggle, Anthropic, NVIDIA DLI) on a phone.
I build to learn. This is project v2 — rebuilt from scratch after I lost v1 to a
broken phone. The builder survived. ⚡

---
*Built with curiosity, Python, and a stubborn belief that Africa's problems
deserve home-grown, data-driven answers.* 🇬
