```
```python
class PredictionRequest(BaseModel):
    features: list # Adjust the request model based on your feature set

@app.post("/predict")
async def predict(request: PredictionRequest):
    feature_df = spark.createDataFrame(
        [tuple(request.features)],
        ["feature1", "feature2", "feature3"]) # Adjust schema
    accordingly
    prediction = model.transform(feature_df)
    prediction_result = prediction.collect()[0]["prediction"]
    return {"prediction": prediction_result}
```
```

+   5. **Containerize the application:** To deploy the application, it’s a good practice to containerize it using Docker. This makes the deployment environment agnostic and simplifies scaling. Create a Dockerfile in the same directory as your FastAPI app:

```
```dockerfile
# Use an official Python runtime as a parent image
FROM python:3.8-slim
# Set the working directory in the container
WORKDIR /usr/src/app

# Copy the current directory contents into the container at /
usr/src/app
COPY . .
# Install any needed packages specified in requirements.txt
RUN pip install --no-cache-dir -r requirements.txt
# Make port 5000 available to the world outside this container
EXPOSE 5000
# Define environment variable
ENV NAME World
# Run app.py when the container launches
CMD ["python", "app.py"]
```
```

Make sure you have a requirements.txt file that includes Flask and PySpark:

```
```
flask
```
```

Build and run your Docker container:

```
```bash
docker build -t spark-model-api .
docker run -p 4000:5000 spark-model-api
```
```

+   6. **Expose the endpoint:** Once your API is set up, expose the /predict endpoint to accept feature data and return predictions. Ensure that the API is properly secured and authenticated if required.