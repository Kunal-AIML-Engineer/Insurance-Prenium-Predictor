# 🏥 Insurance Premium Category Predictor

An end-to-end Machine Learning web application that predicts insurance premium categories (**Low**, **Medium**, **High**) based on user demographics, physical parameters, financial standing, and lifestyle risk factors. 

The project uses a **Random Forest Classifier** wrapped in a **Scikit-Learn Pipeline**, served through a **FastAPI** backend REST API, and visualized via an interactive **Streamlit** user interface.

---

## 📌 Table of Contents
- [Overview](#-overview)
- [Key Features](#-key-features)
- [System Architecture](#-system-architecture)
- [Project Directory Structure](#-project-directory-structure)
- [Installation & Setup](#-installation--setup)
- [Running the Application](#-running-the-application)
- [Feature Engineering & Business Logic](#-feature-engineering--business-logic)
- [API Endpoints](#-api-endpoints)
- [Technologies Used](#-technologies-used)


---

## ℹ️ Overview

Determining health and life insurance premium risk tiers accurately is crucial for underwriting. This application automates the prediction process by engineering meaningful indicators—such as Body Mass Index (BMI), lifestyle risk scores, age categorization, and city tier classifications—from basic raw user inputs.

---

## ✨ Key Features

- **Automated Feature Engineering**: Dynamic calculation of BMI, Lifestyle Risk (`low`, `medium`, `high`), Age Group (`young`, `adult`, `middle_aged`, `senior`), and City Tier (`Tier 1`, `Tier 2`, `Tier 3`).
- **RESTful API Backend**: Built with FastAPI using Pydantic models for request validation and type safety.
- **Interactive UI**: A clean, intuitive Web Interface powered by Streamlit for instant predictions.
- **Production-Ready Pipeline**: Preprocessing (OneHotEncoder + Pass-through) and RandomForest model packaged cleanly using Scikit-Learn `Pipeline`.

---

## 📐 System Architecture

```
┌─────────────────┐        HTTP POST        ┌─────────────────┐
│                 │  (/predict payload)     │                 │
│  Streamlit UI   │ ──────────────────────> │   FastAPI App   │
│  (frontend.py)  │ <────────────────────── │    (app.py)     │
└─────────────────┘      JSON Response      └────────┬────────┘
                                                     │
                                             Loads & Evaluates
                                                     │
                                                     ▼
                                            ┌─────────────────┐
                                            │    model.pkl    │
                                            │  (RF Pipeline)  │
                                            └─────────────────┘
```

---

## 📁 Project Directory Structure

```text
insurance-category-predictor/
├── app.py              # FastAPI REST API backend
├── frontend.py         # Streamlit web user interface
├── train.py            # Model training & pipeline export script
├── insurance.csv       # Training dataset
├── model.pkl           # Serialized Scikit-Learn pipeline
├── requirements.txt    # Project dependencies
└── README.md           # Project documentation
```

---

## ⚙️ Installation & Setup

### Prerequisites
- Python 3.9 or higher
- `pip` package manager

### 1. Clone the Repository
```bash
git clone https://github.com/your-username/insurance-category-predictor.git
cd insurance-category-predictor
```

### 2. Create a Virtual Environment
```bash
# macOS/Linux
python3 -m venv venv
source venv/bin/activate

# Windows
python -m venv venv
venv\Scripts\activate
```

### 3. Install Dependencies
```bash
pip install -r requirements.txt
```

---

## 🚀 Running the Application

### Step 1: Train/Export the ML Model (Optional)
If `model.pkl` is missing or you want to retrain the model on updated data:
```bash
python train.py
```

### Step 2: Launch the FastAPI Backend Server
Start the Uvicorn server hosting the FastAPI service at `http://127.0.0.1:8000`:
```bash
uvicorn app:app --reload
```
> 💡 Interactive Swagger API docs are accessible at `http://127.0.0.1:8000/docs`.

### Step 3: Launch the Streamlit Frontend UI
In a separate terminal window (with virtual environment activated):
```bash
streamlit run frontend.py
```
> 🌐 The Streamlit UI will automatically open at `http://localhost:8501`.

---

## 🧠 Feature Engineering & Business Logic

| Feature | Input Parameters | Derived Logic / Categories |
| :--- | :--- | :--- |
| **BMI** | Height ($m$), Weight ($kg$) | $	ext{BMI} = rac{	ext{Weight}}{	ext{Height}^2}$ |
| **Age Group** | Age (years) | `young` (<25), `adult` (<45), `middle_aged` (<60), `senior` (60+) |
| **Lifestyle Risk** | Smoker status, BMI | • **High**: Smoker & BMI > 30<br>• **Medium**: Smoker OR BMI > 27<br>• **Low**: Otherwise |
| **City Tier** | City Name | • **Tier 1**: Major Metros (Mumbai, Delhi, Bangalore, etc.)<br>• **Tier 2**: Tier-2 Cities (Jaipur, Indore, Lucknow, etc.)<br>• **Tier 3**: Other regions |

---

## 📡 API Endpoints

### `POST /predict`
Accepts user attributes and returns the predicted insurance premium category.

**Sample Request Body:**
```json
{
  "age": 35,
  "weight": 75.0,
  "height": 1.75,
  "income_lpa": 12.5,
  "smoker": false,
  "city": "Mumbai",
  "occupation": "private_job"
}
```

**Sample Response:**
```json
{
  "predicted_category": "Medium"
}
```

---

## 🛠️ Technologies Used

- **Language**: Python 3.12+
- **Machine Learning**: Scikit-Learn, Pandas, NumPy
- **Backend API**: FastAPI, Uvicorn, Pydantic
- **Frontend UI**: Streamlit, Requests


