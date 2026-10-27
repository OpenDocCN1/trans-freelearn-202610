# Federated learning (Privacy-Preserving Deep Learning) for distributed image classification

Traditional deep learning assumes that all training data is collected and stored on a central server. In many real-world applications such as hospitals, mobile devices, banks, and autonomous vehicles, sharing raw data may be impossible due to privacy, legal, or security constraints.

Federated Learning (FL) addresses this challenge by allowing multiple clients to collaboratively train a model without sharing their local data. Instead of transmitting images, each client trains a local model and sends only model updates (weights or gradients) to a central server.

Server ← Model Updates ← Clients

The server aggregates the updates and distributes the improved global model back to all clients.

Mathematical Formulation: Assume there are K clients. Client k owns local dataset D_k = (x_i, y_i) and minimizes its local objective:

F_k(w) = 1/|D_k| Σ_{(x,y)∈D_k} L(f(x; w), y)

where w = network parameters, L = classification loss, f = CNN model.

The global optimization problem becomes:

F(w) = Σ_{k=1}^K |D_k|/N F_k(w)

where N = Σ_{k=1}^K |D_k| is the total number of samples.

Federated Averaging (FedAvg): The most widely used federated learning algorithm is Federated Averaging (FedAvg). Each client trains locally and produces weights w_1, w_2, ..., w_K. The server computes w_global = 1/K Σ_{k=1}^K w_k and redistributes the averaged model. This simple averaging procedure allows all clients to benefit from collective learning while keeping their data private.

Imagine five hospitals (A, B, C, D, E) each owning medical images. Hospital A, B, and C never share images. Instead, each hospital trains a local CNN and sends only learned parameters to a central server. The server averages the parameters and creates a stronger global model that has indirectly learned from all hospitals without ever seeing their raw data.

Dataset: Fashion-MNIST: The demonstration uses the handwritten fashion dataset: 28 × 28 grayscale clothing images belonging to ten classes: T-Shirt, Trouser, Pullover, Dress, Coat, Sandal, Shirt, Sneaker, Bag and Ankle Boot. The dataset is randomly divided among five simulated clients.

```
num_clients = 5
client_indices = np.array_split(indices, num_clients)
```

Each client receives its own private subset.