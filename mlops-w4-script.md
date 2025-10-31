### 🎥 **Video Recording Script** 🎥

*(Start with your MLflow UI open in your laptop browser)*

"Hi, this is my Week 5 demo. I've already completed all the steps, so I'll walk you through the final, working system."

"The goal was to integrate **MLflow** into our pipeline. My approach was to set up a central server, run hyperparameter tuning experiments, and then have `pytest` pull the best model from the MLflow registry for validation."

---

### ☁️ Cloud Setup

*(Show your GCP Console & SSH Terminal)*

"First, the setup. My MLflow server is running 24/7 on my **Vertex AI Workbench**. I'm in the jupyter terminal now, and you can see the server is running inside this `screen` session using the `mlflow server ... --host 0.0.0.0` command."

"In the GCP Console, this **firewall rule** (`allow-mlflow-access`) opens **TCP port 5000**. This is what lets me access the UI you see here, directly from my laptop's browser."

---

### 🏃‍♂️ Actions & Results

*(Show your JupyterLab tab)*

"The input data was the same **`iris.csv`** file. I first ran my new **`train_hpt.py`** script."

*(Switch to the MLflow UI, "Experiments" tab)*

"Running that script produced these two experiments in the 'Iris\_HPT\_Experiment'. We were testing C-values of 0.1 and 1.0. When I compare them, you can see the run with **`C=1.0`** gave a perfect accuracy, making it the clear winner."

*(Click the "Models" tab in the MLflow UI)*

"So, I went to the **Models** tab, found that version, and, as you can see, I have already **promoted it to 'Production'**."

*(Switch to your JupyterLab Terminal, showing the `pytest` output)*

"Finally, I ran `pytest -v`. As you can see from the output, it **passed**. This test successfully connected to the MLflow server, downloaded that *specific 'Production'* model, and validated it."

---

### Code Explanation

*(Show the code for `train_hpt.py` and `test_model.py`)*

"This was all driven by two key scripts:
1.  **`train_hpt.py`**: This script logs parameters, metrics, and most importantly, registers the model using `mlflow.sklearn.log_model`.
2.  **`test_model.py`**: This script was updated to no longer use a local file. It now fetches the model using this URI: **`models:/iris_classifier/Production`**. This makes our testing completely dynamic."

---

### ⚠️ Errors & Learnings

"I did hit a 'Connection Refused' error. I fixed it by adding the `MLFLOW_TRACKING_URI` to `test_model.py` so it knew where the server was.

My main learning is how MLflow's **Model Registry decouples** training from production. My training script can run 100 experiments, and my test script doesn't care. It just asks the registry for the 'Production' model. This is a much more robust MLOps workflow."

"This concludes my demo. Thank you."
