# Health Prediction API

A Flask-based REST API that provides machine learning predictions for diabetes and heart disease using trained models.

## Features

- **Diabetes Prediction**: Uses Support Vector Machine (SVM) with linear kernel
- **Heart Disease Prediction**: Uses Logistic Regression
- **RESTful API**: Easy-to-use endpoints for predictions
- **Data Preprocessing**: Automatic scaling and normalization
- **Error Handling**: Comprehensive error responses

## Project Structure

```
project/
├── main.py                          # Application entry point
├── app/
│   ├── __init__.py                  # Flask app factory
│   ├── routes/
│   │   ├── diabetes.py              # Diabetes prediction routes
│   │   └── heart.py                 # Heart disease prediction routes
│   └── models/
│       ├── diabetes_model.py        # Diabetes ML model
│       ├── heart_model.py           # Heart disease ML model
│       └── dataset/
│           ├── diabetes.csv         # Diabetes training data
│           └── heart_disease_data.csv # Heart disease training data
└── requirements.txt                 # Python dependencies
```

## Prerequisites

- Python 3.7 or higher
- pip (Python package installer)

## Installation & Setup

### 1. Clone the Repository

```bash
git clone <repository-url>
cd health-prediction-api
```

### 2. Create Virtual Environment (Recommended)

```bash
# Create virtual environment
python -m venv venv

# Activate virtual environment
# On Windows:
venv\Scripts\activate
# On macOS/Linux:
source venv/bin/activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Prepare Dataset Files

Ensure you have the following CSV files in the `app/models/dataset/` directory:

- `diabetes.csv` - Diabetes dataset with features and 'Outcome' column
- `heart_disease_data.csv` - Heart disease dataset with features and 'target' column

**Expected Diabetes Dataset Features:**
- Pregnancies
- Glucose
- BloodPressure
- SkinThickness
- Insulin
- BMI
- DiabetesPedigreeFunction
- Age
- Outcome (target variable)

**Expected Heart Disease Dataset Features:**
Standard heart disease prediction features with 'target' as the outcome variable.

### 5. Run the Application

```bash
python main.py
```

The API will start running on `http://127.0.0.1:5000` in debug mode.

## API Endpoints

### 1. Diabetes Prediction

**Endpoint:** `POST /predict/diabetes`

**Request Format:**
```json
{
  "features": [pregnancies, glucose, blood_pressure, skin_thickness, insulin, bmi, diabetes_pedigree_function, age]
}
```

**Example Request:**
```json
{
  "features": [6, 148, 72, 35, 0, 33.6, 0.627, 50]
}
```

**Response:**
```json
{
  "is_diabetic": true
}
```

### 2. Heart Disease Prediction

**Endpoint:** `POST /predict/heart`

**Request Format:**
```json
{
  "features": [age, sex, cp, trestbps, chol, fbs, restecg, thalach, exang, oldpeak, slope, ca, thal]
}
```

**Example Request:**
```json
{
  "features": [63, 1, 3, 145, 233, 1, 0, 150, 0, 2.3, 0, 0, 1]
}
```

**Response:**
```json
{
  "has_disease": false
}
```

## Testing the API

### Using cURL

**Test Diabetes Prediction:**
```bash
curl -X POST http://127.0.0.1:5000/predict/diabetes \
  -H "Content-Type: application/json" \
  -d '{"features": [6, 148, 72, 35, 0, 33.6, 0.627, 50]}'
```

**Test Heart Disease Prediction:**
```bash
curl -X POST http://127.0.0.1:5000/predict/heart \
  -H "Content-Type: application/json" \
  -d '{"features": [63, 1, 3, 145, 233, 1, 0, 150, 0, 2.3, 0, 0, 1]}'
```

### Using Python Requests

```python
import requests
import json

# Diabetes prediction
diabetes_data = {
    "features": [6, 148, 72, 35, 0, 33.6, 0.627, 50]
}
response = requests.post('http://127.0.0.1:5000/predict/diabetes', 
                        json=diabetes_data)
print(response.json())

# Heart disease prediction
heart_data = {
    "features": [63, 1, 3, 145, 233, 1, 0, 150, 0, 2.3, 0, 0, 1]
}
response = requests.post('http://127.0.0.1:5000/predict/heart', 
                        json=heart_data)
print(response.json())
```

## Error Handling

The API returns appropriate HTTP status codes and error messages:

**400 Bad Request Example:**
```json
{
  "error": "Missing required features"
}
```

Common error scenarios:
- Missing or invalid JSON data
- Incorrect number of features
- Invalid feature values
- Model prediction errors

## Model Details

### Diabetes Model
- **Algorithm**: Support Vector Machine (SVM) with linear kernel
- **Preprocessing**: StandardScaler normalization
- **Features**: 8 input features
- **Output**: Boolean (True = Diabetic, False = Non-diabetic)

### Heart Disease Model
- **Algorithm**: Logistic Regression (max_iter=2000)
- **Preprocessing**: None (raw features used)
- **Features**: 13 input features
- **Output**: Boolean (True = Has disease, False = No disease)

## Development

### Adding New Models

1. Create a new model file in `app/models/`
2. Implement prediction function
3. Create route blueprint in `app/routes/`
4. Register blueprint in `app/__init__.py`

### Model Training

Models are automatically trained when the modules are imported. Training uses:
- 80% training data, 20% test data
- Stratified splitting to maintain class balance
- Random state = 2 for reproducibility

## Production Deployment

For production deployment:

1. **Set Debug Mode Off:**
   ```python
   app.run(debug=False, host='0.0.0.0', port=5000)
   ```

2. **Use Production WSGI Server:**
   ```bash
   pip install gunicorn
   gunicorn -w 4 -b 0.0.0.0:5000 main:app
   ```

3. **Environment Variables:**
   - Set `FLASK_ENV=production`
   - Configure proper logging
   - Use environment-specific configurations

## Troubleshooting

### Common Issues

1. **Import Errors**: Ensure all dependencies are installed
2. **File Not Found**: Check dataset file paths
3. **Memory Issues**: Large datasets may require more RAM
4. **Port Already in Use**: Change port in main.py or kill existing process

### Logs

Check console output for detailed error messages and request logs.

## Contributing

1. Fork the repository
2. Create feature branch
3. Make changes
4. Add tests
5. Submit pull request

## License

This project is licensed under the MIT License.

## Support

For issues and questions:
- Check the troubleshooting section
- Review error logs
- Open an issue in the repository
