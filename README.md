# MLB Statcast Postseason Pitching Analysis (2025)

This project combines **data science** and **scouting-style evaluation** to analyze MLB postseason pitching using Statcast pitch-by-pitch tracking data.

Using Python, Pandas, and Jupyter, I built an end-to-end workflow that:
- Cleans and engineers Statcast data into usable metrics
- Quantifies **velocity**, **whiff ability**, and **hard-hit suppression**
- Produces visual scouting reports on which pitchers dominate and which ones quietly prevent damage

---

## 🔍 Project Goals

1. **Analytics Goal:**  
   Build reproducible metrics that describe how postseason pitchers create outs:
   - How hard they throw
   - How often they generate swings-and-misses
   - How well they limit quality contact (95+ mph)

2. **Scouting Goal:**  
   Translate those metrics into **scouting-style insights**:
   - Identify “power arms” vs “weak-contact specialists”
   - Highlight which pitch types function as true out pitches
   - Inform hypothetical decisions on **who you’d trust in high-leverage playoff innings**

---

## 📊 Data & Tools

**Dataset**

- Source: Statcast pitch-by-pitch data for the **2025 MLB Postseason**  
- Each row = a single pitch event (pitch type, velocity, spin, location, outcome, etc.)
- Key columns used:
  - `pitch_type`, `release_speed`, `release_spin_rate`
  - `launch_speed`, `launch_angle`
  - `events`, `description`
  - `is_swing`, `is_whiff`, `is_hard_hit` (engineered features)

**Tech Stack**

- Python 3  
- Jupyter Notebook  
- `pandas`, `numpy`  
- `matplotlib`, `seaborn`  

---

## 📐 Methods & Metrics

The analysis is organized into three phases, mirroring how teams evaluate pitchers:

### Phase 1 – Pitch Arsenal & Velocity Profile

- Compute average **velocity by pitch type** (FF, SI, SL, CH, CU, etc.).
- Rank pitchers by maximum and mean fastball velocity.
- Characterize arsenals:
  - Power fastball + power breaker
  - Sinker/changeup contact managers
  - Mixed arsenals with multiple usable secondaries

**Scouting lens:**  
Who has “big league power stuff” versus who relies more on mix, command, and movement?

---

### Phase 2 – Spin & Whiff Ability (Swing–Miss Pitching)

- Engineer `is_swing` and `is_whiff` flags from Statcast `description`.
- Calculate **whiff rate by pitch type** and **by pitcher**:
  \[
  \text{Whiff Rate} = \frac{\text{whiffs}}{\text{swings}}
  \]
- Connect spin/movement to whiff performance rather than just looking at raw RPM.
- Filter out tiny samples (e.g., minimum ~20 swings).

**Scouting lens:**  
Which pitches are true **out pitches**?  
Which pitchers have the movement profile and deception to miss bats in the zone and above it?

---

### Phase 3 – Contact Quality & Hard-Hit Suppression

- Use batted-ball events with `launch_speed` to define:
  - `is_hard_hit` = 1 if **exit velocity ≥ 95 mph**, else 0
- Compute **hard-hit rate allowed by pitch type** and **by pitcher**.
- Apply a **minimum 30 balls in play (BIP)** filter to avoid noisy leaders.
- Visualize “Lowest Hard-Hit Rate Allowed – Top 15 Pitchers (2025 Postseason)” in `figures/hard_hit_suppression_chart.png`.

**Scouting lens:**  
Identify **weak-contact specialists** who may not have huge K numbers but consistently avoid barrels and loud contact.

---

## 🔑 Key Findings (Hybrid Scouting + Analytics)

- **Velocity still matters**, but it’s not the full story. Power fastballs set up secondaries, but several non-elite velocity arms succeed by pairing average velo with plus movement.
- **Changeups, sliders, and curveballs** drive the highest whiff rates when they show strong velocity separation and late movement, even without top-of-the-scale spin.
- Some pitchers quietly post **elite hard-hit suppression** with sinkers and off-speed pitches, functioning as “damage control” arms who keep the ball in the yard and limit extra-base hits.
- The most playoff-ready pitchers blended:
  1. **Enough velocity** to compete in the zone  
  2. **At least one true swing-and-miss pitch**  
  3. **Contact-management ability** to avoid barrels when hitters do connect  

This mirrors how front offices and analysts now build bullpens: a mix of power strikeout arms and contact suppressors.

---

## 📁 Repository Contents

```text
notebooks/
  statcast_postseason_analysis.ipynb   # Main analysis notebook

reports/
  statcast_postseason_analysis.html    # Rendered report view (no code required)

figures/
  hard_hit_suppression_chart.png      # Key visualization for portfolio / LinkedIn

data/
  Data_MLB_2025_StatcastPostseason_PitchByPitch_20251102a.csv  # Raw Statcast export (if licensing allows)
