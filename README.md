# IPL Live Win Probability Predictor (WASP) 🏏

An end-to-end Machine Learning pipeline that dynamically predicts in-game IPL (Indian Premier League) win probabilities. This project acts as a functional WASP (Win and Score Predictor), similar to the live broadcast graphics used in modern T20 cricket.

## 📌 Project Overview
Predicting the outcome of a T20 match before it starts is inherently flawed due to the dynamic nature of the game. This project solves that by evaluating the **live, ball-by-ball match state** during a run chase. 

Initially built using XGBoost, the engine was migrated to **Logistic Regression** and scaled using `StandardScaler` to correct a probability calibration issue (Concept Drift) caused by the modern "Impact Player" era, ensuring mathematically sound percentage outputs for extreme edge cases (e.g., needing 20 runs off 6 balls).

### Core Technologies
* **Language:** Python
* **Data Processing:** Pandas, NumPy
* **Machine Learning:** Scikit-Learn (Logistic Regression, StandardScaler)
* **Deployment:** Google Colaboratory

---

## 🧠 Advanced Feature Engineering
To make the model context-aware of modern high-scoring T20 dynamics, the following custom features were engineered from the raw ball-by-ball data:
* **Dynamic Run Rates:** Calculated Current Run Rate (CRR) and Required Run Rate (RRR) dynamically for every ball.
* **Rolling 5-Year Era Filter:** Automatically subsets the dataset to the most recent 5 years of data to account for modern scoring inflation and rule changes.
* **`is_death_overs`:** A binary flag alerting the model when the match enters the final 30 balls.
* **`wickets_x_balls`:** An interaction feature that mathematically teaches the model the exponential value of having wickets in hand during the final overs.

---

## 🚀 How to Run and Test the Project (Step-by-Step)

This project is hosted on Google Colab for one-click reproducibility. No local installation or dataset downloading is required.

1. **Open the Notebook:** Click the blue `Open in Colab` badge at the top of the `.ipynb` file in this repository.
2. **Run the Pipeline:** In Google Colab, go to the top menu and click **Runtime > Run all**.
3. **Wait for Training:** The notebook will automatically fetch the compressed dataset from this repository, unzip it in memory, engineer the features, and train the model.
4. **View the Output:** Scroll to the bottom cell to see the live broadcast-style terminal output.

### 🎛️ Editing Match Scenarios (Interactive Testing)
You can test the model against any hypothetical or live IPL match situation. 
1. Scroll to the very last cell in the Colab notebook.
2. Locate the function call: `predict_win_probability(target_score=..., current_score=..., wickets_lost=..., overs_completed=...)`
3. Change the numbers to your desired scenario. 
   * *Example:* For a team chasing 200, currently at 150/4 after 14.2 overs:
     `predict_win_probability(target_score=200, current_score=150, wickets_lost=4, overs_completed=14.2)`
4. Press `Shift + Enter` to re-run that specific cell and view the updated win probability.

---

## 🔄 Future-Proofing: Updating the Dataset
This pipeline is designed to be easily updated for future IPL seasons without breaking the code.

**To update the dataset:**
1. Download the latest ball-by-ball IPL dataset from Kaggle (CSV format).
2. Compress the `CSV` file into a `.zip` format (e.g., `IPLLast5YearData.zip`). *This is crucial to bypass GitHub's 25MB file limit.*
3. Upload and replace the existing `.zip` file in this repository.
4. **No code changes are required.** The pipeline dynamically reads the `year` column and will automatically slide its 5-year training window forward to accommodate the new data.

---

## ⚠️ Known Limitations
While highly accurate for general match states, this model has structural limitations inherent to its feature set:
* **Pitch & Weather Agnostic:** The model does not factor in dew, pitch degradation, or ground dimensions. A required run rate of 12 at Eden Gardens is treated identically to a required run rate of 12 at Chepauk.
* **Player Neutrality:** The model evaluates the overall match state, not the individuals at the crease. It does not know if Jasprit Bumrah is bowling the final over or if MS Dhoni is batting.
* **Toss & First Innings Excluded:** This is strictly a 2nd-innings run-chase calculator. It does not project 1st innings scores.

---

## 👨‍💻 Author
**Suvam Paul** * Built as a portfolio project demonstrating data engineering, model selection, and probability calibration.

## 📄 License & Data Source
* **License:** [MIT License](LICENSE)
* **Data Source:** Historical ball-by-ball IPL dataset sourced from Kaggle. All original data rights belong to their respective curators. This project is strictly for educational and portfolio purposes.
