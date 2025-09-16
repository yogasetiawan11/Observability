# Prerequisites
Before we dive into the deployment process, ensure you have the following prerequisites:

- A running Kubernetes cluster (Minikube in this case).
- Kubectl installed and configured to interact with your Kubernetes cluster.
- Helm, the package manager for Kubernetes, installed.
- Step-by-Step Deployment

# Step 1: Create a Namespace
Namespaces in Kubernetes provide a way to divide cluster resources between multiple users. We’ll create a namespace for our monitoring tools.

```bash
kubectl create namespace monitoring
```
# Step 2: Prometheus Configuration
Create a configuration file for Prometheus. This configuration will define how Prometheus scrapes metrics from the nodes in the cluster.

- Create a file named prometheus-config.yaml with the following content:
```bash
apiVersion: v1
kind: ConfigMap
metadata:
  name: prometheus-server-conf
  namespace: monitoring
data:
  prometheus.yml: |
    global:
      scrape_interval: 15s
      evaluation_interval: 15s
    scrape_configs:
      - job_name: 'prometheus'
        static_configs:
          - targets: ['localhost:9090']
```
- Apply the configuration:
```bash
kubectl apply -f prometheus-config.yaml -n monitoring
```
# Step 3: Prometheus Deployment
Next, create a deployment file for Prometheus. This deployment will use the ConfigMap we just created.

- Create a file named prometheus-deployment.yaml:
```bash
apiVersion: apps/v1
kind: Deployment
metadata:
  name: prometheus-server
  namespace: monitoring
spec:
  replicas: 1
  selector:
    matchLabels:
      app: prometheus-server
  template:
    metadata:
      labels:
        app: prometheus-server
    spec:
      containers:
        - name: prometheus
          image: prom/prometheus
          ports:
            - containerPort: 9090
          volumeMounts:
            - name: config-volume
              mountPath: /etc/prometheus
      volumes:
        - name: config-volume
          configMap:
            name: prometheus-server-conf
            defaultMode: 420
Apply the deployment:
```

```bash
kubectl apply -f prometheus-deployment.yaml -n monitoring
```
# Step 4: Expose Prometheus as a Service
To access Prometheus, we need to expose it as a service.

- Create a file named prometheus-service.yaml:
```bash
apiVersion: v1
kind: Service
metadata:
  name: prometheus-service
  namespace: monitoring
spec:
  selector:
    app: prometheus-server
  ports:
    - protocol: TCP
      port: 80
      targetPort: 9090
  type: LoadBalancer
```

- **Apply the service:**
```bash
kubectl apply -f prometheus-service.yaml -n monitoring
```

# Step 5: Install Grafana
We’ll use Helm to install Grafana. First, add the Grafana Helm repository and update it.
```bash
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
```
- Install Grafana in the monitoring namespace:
```bash
helm install grafana grafana/grafana --namespace monitoring
Check the status of the pods to ensure Grafana is running:
```
- Verify Grafana Installed
```bash
kubectl get pods -n monitoring
```
- Step 6: Access Grafana
Having successfully installed Grafana on the Kubernetes Cluster, the Grafana server is now accessible through port 80. To retrieve the complete list of Kubernetes Services associated with Grafana, execute the following command:
```bash
kubectl get service
```
Output:

kubectl get service
NAME                                  TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
grafana                               ClusterIP   10.104.22.18    <none>        80/TCP         4m6s

- Exposing the Grafana Kubernetes Service
To expose the Grafana-service on Kubernetes we need to run this command
```bash
kubectl expose service grafana --type=NodePort --target-port=3000 --name=grafana-ext -n monitoring
```
Press enter or click to view image in full size

By executing this command, we transition the service type from ClusterIP to NodePort, enabling external access to Grafana beyond the Kubernetes Cluster. The service will now be reachable on port 3000.

- This below command will generate the following URL to access the Grafana dashboard

```bash
minikube service grafana-ext
```

<img width="1099" height="671" alt="Image" src="https://github.com/user-attachments/assets/17fad616-e739-49ef-8675-93cc7ab3ce89" />

If this command executed You can copy the IP address then head to browser and access Grafana.