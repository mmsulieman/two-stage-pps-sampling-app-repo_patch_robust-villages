# Two-Stage PPS Sampling App (WFP Somali Region)

[![Open in Streamlit](https://static.streamlit.io/badges/streamlit_badge.svg)](https://streamlit.io/cloud)

A deployable Streamlit app implementing your **Jijiga AO** spec.

---

## 📸 Screenshots
![Home](screenshots/01_home.png)
![Sidebar](screenshots/02_sidebar.png)
![Selection & Allocation](screenshots/03_selection_allocation.png)
![Export](screenshots/04_export.png)

---

## 🧭 Step-by-Step User Instructions

### 1) Run locally
```bash
pip install -r requirements.txt
streamlit run app.py
```

### 2) Deploy on Streamlit Community Cloud
- Repo entrypoint: **`two-stage-pps-sampling-app/app.py`**
- Select **Python 3.11** in Advanced settings (recommended).

### 3) Prepare input data
Your Excel must have two sheets:
- **`Frame_Villages`** → `Kebele`, `Village`, `E_count`, `I_count`
  - **Accepted variants**: `Frame Villages`, `Frame-Villages`, `Villages`, `Frame_Village`
  - Aliases: `Eligible/Ineligible` or `E/I` → auto‑mapped.
- **`Frame_HHs`** → `Kebele`, `Village`, `Group` *(E/I)*, `HH_ID` + optional fields (`Head_Name`, `Phone`, `Address`)

Use the sample file: **`sample_data/Sampling_Input_Template.xlsx`**.

### 4) Configure sidebar settings
- MOS: `min(E,I)` or `√(E×I)`
- Max villages per kebele (default 2)
- Seed mode: Data-derived / Fixed / Manual
- Replacement rate (%) (default 25)
- MoDa fields & Printable workbook toggles

### 5) Run sampling & export
- Upload Excel → **Run Sampling** → download the Excel with **5 sheets** (+ printable kebele sheets if enabled)

---


## ❓ FAQ

**Q1: How do I ensure reproducibility of sampling results?**  
Use **Seed mode = Data-derived (best)** in the sidebar. This generates a stable seed from your input data, so anyone using the same file and settings gets identical results. Alternatively, choose **Fixed (20251031)** or enter a **Manual seed**.

**Q2: What happens if a kebele has fewer than 30 Ineligible households?**  
The app automatically **skips Ineligible** for that kebele. If one village can host all 30 Eligible households, it allocates all to that single village.

**Q3: What if there aren’t enough households for the target?**  
Targets are capped at available households. The app **rebalances within the kebele** to reach **30 per group** where possible.

**Q4: How are replacements calculated?**  
Replacements = `ceil(replacement_rate × primaries)` per cell, drawn systematically from the remainder **without overlap**. The replacement rate is configurable in the sidebar (default = **25%**).

**Q5: What are MoDa fields?**  
Optional columns (`Attempt`, `Contact_Status`, `Outcome`, `Enumerator`, `Notes`) added blank for integration with **MoDa** tracking.

**Q6: What does the Excel export include?**  
Sheets: `Primaries`, `Replacements`, `Selected_Villages`, `Allocation_Summary`, `Settings_Log` — plus optional **printable sheets per kebele** for field teams.

**Q7: When does PPS use fallback MOS?**  
Default MOS is **`min(E,I)`**; you may toggle **`√(E×I)`**. If **all `min(E,I)=0`** in a kebele, the app **falls back to `E+I`**.

**Q8: Can I change the maximum villages per kebele?**  
Yes — adjust **Max villages per kebele** in the sidebar (default **2**).

**Q9: Does the app handle zero-MOS villages or empty cells?**  
Yes. Villages with zero MOS are excluded unless fallback applies. Empty cells yield **0 primaries/replacements** and appear cleanly in outputs.

**Q10: What’s the best replacement rate for field practicality?**  
Default **25%** works well (8 replacements for 30 primaries). For hard-to-reach sites, consider **33%** or **50%**, but ensure enough remainder HHs exist.


---

## 📂 Repo Structure
```
two-stage-pps-sampling-app/
├─ app.py
├─ requirements.txt
├─ README.md
├─ LICENSE
├─ .gitignore
├─ screenshots/
│  ├─ 01_home.png
│  ├─ 02_sidebar.png
│  ├─ 03_selection_allocation.png
│  └─ 04_export.png
└─ sample_data/
   └─ Sampling_Input_Template.xlsx
```
