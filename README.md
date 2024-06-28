from flask import Flask, request, jsonify
import joblib
import pandas as pd

app = Flask(_name_)

# Load the pre-trained model
model = joblib.load('fraud_detection_model.pkl')

@app.route('/predict', methods=['POST'])
def predict():
    data = request.json
    df = pd.DataFrame([data])
    
    # Ensure the order of columns matches the training data
    columns = ['feature1', 'feature2', 'feature3']  # Replace with actual feature names
    df = df[columns]

    prediction = model.predict(df)
    return jsonify({'fraud': bool(prediction[0])})

if _name_ == '_main_':
    app.run(debug=True)
