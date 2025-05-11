##  AI Intrusion Detection 

## Dataset used
  -Source: CIC-IDS2017
  -Total Classes: 15 (BENIGN + 14 attacks)
  -Encoding: Used LabelEncoder to convert string labels into numeric codes
  -Binary Labels: For anomaly detection:
                  0 = BENIGN
                  1 = Attack (all others)
## Dataset Splitting

```
# Stratified Splits
train_val, test = 80/20
train, val = 60/20
```

  - Stratified using `train_test_split()` to preserve class distribution
    
  - Separate splits for supervised (RF) and unsupervised (IF) models

## Types of ML Models used
  ##Supervised learning
    -Random Forest
  - **Model**: `RandomForestClassifier(n_estimators=100, class_weight='balanced')`
      
  - **Training**: `fit(X_train, y_train)`
      
  - **Evaluation**:
      
      - `classification_report`
          
      - `confusion_matrix`
          
      - Excellent accuracy on BENIGN
          
      - Weak recall on rare attacks
    
  ##Unsupervised learning
    -Isolation Forest
    -
    -Isolation Forest can flag abnormal network traffic that could signify a cyberattack or unauthorized access.
    - **Model**: `IsolationForest(contamination=<computed>, n_estimators=100)`
    
  - **Training**: `fit(X_train)` (no y used)
      
  - **Scoring**:
      
      - Used `decision_function()` → lower = more anomalous
          
      - Flipped scores for ROC curve
          
  - **Evaluation**:
      
      - ROC Curve & AUC Score
          
      - Classification report (with thresholded scores)

  ## Model Saving

```
import joblib

# Save
joblib.dump(rf_model, 'random_forest_model.pkl')
joblib.dump(iso_model, 'isolation_forest_model.pkl')
joblib.dump(scaler, 'scaler.pkl')

# Load
rf_model = joblib.load('random_forest_model.pkl')
iso_model = joblib.load('isolation_forest_model.pkl')
```
