# k8s-project1

## Step 1: Make sure your docker is running and start minikube cluster.

- Check the docker-desktop is running.
- Start minikube cluster using the command `minikube start`

## Step 2: Build the images of ms1, ms2, ms3

- Clone the repo.
- Navigate to the project directory.
- Run the command `docker-compose build` to build the image of microservice1, microservice2, microservice3.

## Step 3: Push these images on dockerhub / ecr

- Login to the docker-hub
- Tag the images with your username and push them to the docker-hub.
- `docker tag microservice1 <username>/<repo>:<tag>`
- `docker push <username>/<repo>:<tag>`

## Step 4: Write ms1-deployment.yml describing the deployment of ms1.
```python
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ms1
spec:
  replicas: 3
  selector:
    matchLabels:
      app: ms1
  template:
    metadata:
      labels:
        app: ms1
    spec:
      containers:
      - name: ms1
        image: ravikishans/microservicenodejs:microservice1
        resources:
          limits:
            memory: "128Mi"
            cpu: "500m"
        ports:
        - containerPort: 3000
```

## Step 5: Write ms1-service.yml describing the service of ms1.
```
apiVersion: v1
kind: Service
metadata:
  name: ms1
spec:
  selector:
    app: ms1
  ports:
  - port: 3000
    targetPort: 3000
    nodePort: 30100
  type: NodePort
```

## Step 6: Replicate these for all the other ms.
- deployment.yml
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ms1
spec:
  replicas: 3
  selector:
    matchLabels:
      app: ms1
  template:
    metadata:
      labels:
        app: ms1
    spec:
      containers:
      - name: ms1
        image: ravikishans/microservicenodejs:microservice1
        resources:
          limits:
            memory: "128Mi"
            cpu: "500m"
        ports:
        - containerPort: 3000

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ms2
spec:
  replicas: 2
  selector:
    matchLabels:
      app: ms2
  template:
    metadata:
      labels:
        app: ms2
    spec:
      containers:
      - name: ms2
        image: ravikishans/microservicenodejs:microservice2
        resources:
          limits:
            memory: "128Mi"
            cpu: "500m"
        ports:
        - containerPort: 3001

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: microserver3-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: ms3
  template:
    metadata:
      labels:
        app: ms3
    spec:
      containers:
      - name: ms3
        image: ravikishans/microservicenodejs:microservice3
        resources:
          limits:
            memory: "128Mi"
            cpu: "500m"
        ports:
        - containerPort: 3002


```
- service.yml
```
apiVersion: v1
kind: Service
metadata:
  name: ms1
spec:
  selector:
    app: ms1
  ports:
  - port: 3000
    targetPort: 3000
    nodePort: 30100
  type: NodePort

---
apiVersion: v1
kind: Service
metadata:
  name: ms2
spec:
  selector:
    app: ms2
  ports:
  - port: 3001
    targetPort: 3001
    nodePort: 30200
  type: NodePort

---

apiVersion: v1
kind: Service
metadata:
  name: ms3
spec:
  selector:
    app: ms3
  ports:
  - port: 3002
    targetPort: 3002
    nodePort: 30300
  type: NodePort


```


## Step 7: Writing the ingress controller.