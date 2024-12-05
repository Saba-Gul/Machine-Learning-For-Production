### MLflow Explained with a Linear Regression Example

MLflow is a comprehensive platform to manage the machine learning lifecycle, including experiment tracking, reproducibility, and deployment of models. Here, we demonstrate its capabilities using a simple linear regression example.

---

### 1. **What is MLflow?**

MLflow has four main modules:

1. **Tracking**: Logs and queries experiments—parameters, metrics, and artifacts.
2. **Projects**: Packages ML code into reusable and reproducible formats.
3. **Models**: Standardizes the model format for deployment.
4. **Model Registry**: Centralized repository for model versioning and lifecycle management.

We’ll explore these concepts, focusing on **Tracking**, **Models**, and **Deployment**, with a detailed walkthrough.

---

### 2. **Key Concepts**

- **Parameters**: Inputs to the model (e.g., algorithm type, hyperparameters).
- **Metrics**: Evaluation results (e.g., mean squared error, accuracy).
- **Artifacts**: Files generated during a run (e.g., model files, plots, configurations).
- **Runs**: Individual executions of an experiment.
- **Model Registry**: Repository for versioning and lifecycle management of models.
- **Deployment Types**:
  - **Local Deployment**: Running the model locally for testing.
  - **Server Deployment**: Deploying via a REST API using MLflow.
  - **Cloud Deployment**: Integrating with platforms like AWS SageMaker, Azure ML, or Google AI Platform.
  - **Kubernetes Deployment**: Deploying models at scale using Kubernetes.

---

### 3. **Linear Regression Example with MLflow**

We’ll use the Boston housing dataset for predicting house prices.

#### **Step 1: Install MLflow**

Before starting, install MLflow:

```bash
pip install mlflow
```

---

#### **Step 2: Set Up the Code**

```python
import mlflow
import mlflow.sklearn
from sklearn.datasets import load_boston
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error

# Load dataset
data = load_boston()
X = data.data
y = data.target

# Split data
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# Start MLflow run
with mlflow.start_run():
    # Model training
    model = LinearRegression()
    model.fit(X_train, y_train)

    # Make predictions
    predictions = model.predict(X_test)

    # Calculate metrics
    mse = mean_squared_error(y_test, predictions)

    # Log parameters, metrics, and model
    mlflow.log_param("model", "LinearRegression")
    mlflow.log_metric("mse", mse)
    mlflow.sklearn.log_model(model, "linear_regression_model")

    print(f"Logged Model with MSE: {mse}")
```

---

#### **Step 3: Viewing Results in the MLflow UI**

1. Run the script.
2. Start the MLflow UI:
   ```bash
   mlflow ui
   ```
3. Open your browser and go to `http://localhost:5000` to see the experiment details (parameters, metrics, and artifacts).

---

### 4. **Analyzing the MLflow Tracking Output**

After running the code:

- **Parameters**: The model type logged as “LinearRegression”.
- **Metrics**: The mean squared error (MSE).
- **Artifacts**: The serialized model saved for reuse.
  - Artifacts may include the model itself, plots (e.g., learning curves), or data samples.

---

### 5. **Deploying the Model**

MLflow simplifies deployment with standardized formats and tools.

#### **Local Deployment**

You can load the saved model and use it for predictions:

```python
import mlflow.sklearn

# Load model
model_uri = "runs:/<run_id>/linear_regression_model"
loaded_model = mlflow.sklearn.load_model(model_uri)

# Make predictions
new_predictions = loaded_model.predict(X_test)
print(new_predictions[:5])
```

Replace `<run_id>` with the actual run ID from your MLflow UI.

#### **Serve the Model Locally**

You can deploy the model using MLflow’s built-in serving feature:

```bash
mlflow models serve -m runs:/<run_id>/linear_regression_model -p 1234
```

Access the model via REST API at `http://localhost:1234/invocations`.

#### **Example REST API Call**

Send data to the model’s API for predictions:

```bash
curl -X POST -H "Content-Type: application/json" \
    -d '{"data": [[6.575, 65.2, 4.0900, 1.0, 296.0, 15.3, 396.9, 4.98, 24.0]]}' \
    http://localhost:1234/invocations
```

---

### 6. **Deploying with Flask and Kubernetes**

#### **Flask Deployment**

You can deploy the model as a REST API using Flask:

```python
from flask import Flask, request, jsonify
import mlflow.sklearn

app = Flask(__name__)
model_uri = "runs:/<run_id>/linear_regression_model"
model = mlflow.sklearn.load_model(model_uri)

@app.route('/predict', methods=['POST'])
def predict():
    data = request.get_json()
    predictions = model.predict(data['data'])
    return jsonify(predictions.tolist())

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

Run the script and access the API at `http://<server_ip>:5000/predict`.

#### **Kubernetes Deployment**

1. **Containerize the Flask App**:
   Create a `Dockerfile`:

   ```dockerfile
   FROM python:3.8-slim

   WORKDIR /app
   COPY . /app

   RUN pip install flask mlflow scikit-learn

   CMD ["python", "app.py"]
   ```

   Build and push the image to a container registry:

   ```bash
   docker build -t <your_docker_repo>/linear_regression_app:latest .
   docker push <your_docker_repo>/linear_regression_app:latest
   ```

2. **Create Kubernetes Deployment and Service**:

   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: linear-regression-deployment
   spec:
     replicas: 2
     selector:
       matchLabels:
         app: linear-regression-app
     template:
       metadata:
         labels:
           app: linear-regression-app
       spec:
         containers:
         - name: linear-regression-container
           image: <your_docker_repo>/linear_regression_app:latest
           ports:
           - containerPort: 5000
   ---
   apiVersion: v1
   kind: Service
   metadata:
     name: linear-regression-service
   spec:
     selector:
       app: linear-regression-app
     ports:
       - protocol: TCP
         port: 80
         targetPort: 5000
     type: LoadBalancer
   ```

   Apply the configuration:

   ```bash
   kubectl apply -f deployment.yaml
   ```

3. **Access the Service**:
   Use the external IP assigned to the LoadBalancer to make API requests.

---

### 7. **Model Registry**

The **Model Registry** allows you to version models and manage their lifecycle (e.g., Staging, Production). You can:

1. **Register a Model**:

   ```bash
   mlflow models register --run-id <run_id> --name linear_regression_model
   ```

2. **Promote a Model**:
   Move models to production or staging environments via the MLflow UI or API.

#### **Model Registry Concepts**

- **Registered Models**: Models assigned a name and version.
- **Stages**:
  - **None**: Default stage for new models.
  - **Staging**: For testing.
  - **Production**: Deployed in production environments.
  - **Archived**: Older versions.

---

### 8. **Benefits of MLflow**

- **Experiment Tracking**: Logs all

= **Artifact Management**: Saves models, visualizations, and related files.

- **Reproducibility**: Stores model, data, and configurations.

- **Ease of Deployment**: Built-in tools for serving models.

- **Model Registry:** Ensures controlled transitions of model versions.

8. Summary

MLflow simplifies the ML lifecycle. This example demonstrated:

- Logging experiments with mlflow.log_* methods.

- Saving and loading models using mlflow.sklearn.

- Serving models via APIs.

- Managing model versions with the Model Registry.

