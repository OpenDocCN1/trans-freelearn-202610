# Start .elser_model_1 deployment

The product of the number of allocations and threads per allocation should be less than the total number of processors on your ML nodes.

Deployment ID
Specify unique identifier for the model deployment.

Priority
Select low priority for demonstrations where each model will be very lightly used.

Number of allocations
Increase to improve throughput of all requests.

Threads per allocation
Increase to improve latency for each request.

Learn more

# Figure 8.6: Starting ELSER

In the preceding screenshot, we set the deployment name to .elser_model_1 and start the model. You should now have a running ELSER model as follows:

# Trained Models

Total trained models: 5

# Figure 8.7: ELSER downloaded and deployed