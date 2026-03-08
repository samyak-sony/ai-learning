# ai-learning
Repo for documenting my ai-learning journey through youtube, articles, and LLMs (mainly qwen)
### 📚 Progress Report: Week 1 (Days 1-4) Summary

You have completed the foundational stack for **Applied AI in SRE**. Below is a detailed summary of every concept we have covered, why it matters, and how you implemented it.

---


#### 3. Interactive Development (Jupyter Lab)
*   **What:** A web-based interface where you run Python code in "cells" and see output immediately.
*   **Why (SRE Context):** **Exploratory Data Analysis (EDA)**. Before building a monitoring dashboard, you need to explore logs interactively to understand patterns.
*   **Example:**
    ```powershell
    jupyter lab  # Opens browser interface
    ```
    *Action:* Press `Shift + Enter` to run a cell.

#### 4. Data Manipulation (Pandas DataFrames)
*   **What:** A 2-dimensional table (rows/columns) stored in memory. Think of it as a programmable Excel sheet or SQL table.
*   **Why (SRE Context):** Logs, metrics, and traces are tabular. Pandas lets you filter, aggregate, and clean this data efficiently.
*   **Example:**
    ```python
    import pandas as pd
    # Create table from dictionary
    df = pd.DataFrame({'timestamp': ['10:00', '10:01'], 'status': ['OK', 'FAIL']})
    # Filter rows
    failures = df[df['status'] == 'FAIL']
    ```

#### 5. Synthetic Data Generation (NumPy)
*   **What:** Using mathematics to create artificial data that mimics real-world distributions.
*   **Why (SRE Context):** You often lack access to production logs due to privacy/security. Synthetic data allows you to build and test tools safely on your local machine.
*   **Example:**
    ```python
    import numpy as np
    # Generate 100 normal values (mean=100, std=20)
    normal = np.random.normal(loc=100, scale=20, size=100)
    # Generate 5 anomalous values (mean=2000)
    anomaly = np.random.normal(loc=2000, scale=50, size=5)
    ```

#### 6. Rule-Based vs. ML-Based Detection
*   **What:**
    *   **Rule-Based:** Hard-coded thresholds (e.g., `if latency > 1000`).
    *   **ML-Based:** Learns the distribution of "normal" and flags deviations.
*   **Why (SRE Context):** Rules cause **alert fatigue** (too many false positives) or **missed incidents** (dynamic baselines change). ML adapts to the data.
*   **Example:**
    *   *Rule:* `df[df['latency'] > 1000]`
    *   *ML:* `model.predict(X)` (Model decides what is abnormal).

#### 7. Anomaly Detection Algorithm (Isolation Forest)
*   **What:** An unsupervised ML algorithm that isolates observations by random splitting. Anomalies are isolated faster (fewer splits).
*   **Why (SRE Context):** Perfect for **unknown unknowns**. You don't need to know what every error looks like; you just need to know what "normal" looks like.
*   **Example:**
    ```python
    from sklearn.ensemble import IsolationForest
    model = IsolationForest(contamination=0.05) # Expect 5% anomalies
    model.fit(X)                                # Learn normal pattern
    preds = model.predict(X)                    # Returns 1 (Normal) or -1 (Anomaly)
    ```

#### 8. Ground Truth & Evaluation Metrics
*   **What:**
    *   **Ground Truth:** The actual labels (created by you in synthetic data).
    *   **Precision:** % of alerts that were real incidents.
    *   **Recall:** % of real incidents that were caught.
*   **Why (SRE Context):**
    *   *Low Precision* = Engineers ignore alerts (Alert Fatigue).
    *   *Low Recall* = Users report outages before you do (Missed SLA).
*   **Example:**
    *   *True Positive (TP):* Model flagged anomaly, it was real.
    *   *False Positive (FP):* Model flagged anomaly, it was normal.
    *   *Precision Formula:* `TP / (TP + FP)`

---
added a local ai agent on my samsung a9 tablet using termux and qwen3.5:0.8b model
the following commands used
1 termux-wake-lock
2 OLLAMA_HOST=0.0.0.0 ollama serve &
3 ollama pull qwen3.5:0.8b
4 ollama run qwen3.5:0.8b
5 curl http://IP:11434/api/generate -d '{
  "model": "qwen3.5:0.8b",
  "prompt": "Explain recursion like I am a beginner",
  "think": false,
  "stream": false
}'
6 docker run -d -p 3000:8080 \
  -e OLLAMA_BASE_URL=http://192.168.1.10:11434 \
  -v open-webui:/app/backend/data \
  --name open-webui \
  --restart always \
  ghcr.io/open-webui/open-webui:main
7 http://localhost:3000
