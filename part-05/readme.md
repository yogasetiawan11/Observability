# Logging 
Logging is crucial for any distribution system, especially for Kubernetes, to monitor application, detect issues, and ensure everything works smothly in Microservices environment. 

# Logging can Help you with
- ``Debugging``: Logs provide crucial information when debugging issues in application.
- ``Security``: Logs help in detecting unauthorized access or malicious activities.
- ``Performance monitoring``: Analyzing can help help to identify the bottlenecks 
- ``Auditing``:  Logs serve as an audit trail, showing what actions were taken and by whom.

# Tools for monitoring Logs
- Elastic FluenD Kibana
- Elastic Fluenbit Logstash Kibana
- Elastic Loggstash Kibana
- Promtail + Loki + Grafana

# EFK Stack (Elasticsearch, Fluentbit, Kibana)
- EFK is a popular logging stack used to collect, store, and analyze logs in Kubernetes.
Elasticsearch: Stores and indexes log data for easy retrieval.
- Fluentbit: A lightweight log forwarder that collects logs from different sources and sends them to Elasticsearch.
- Kibana: A visualization tool that allows users to explore and analyze logs stored in Elasticsearch.


# Set up step by step

1. Create IAM user for Service Account

1) Create IAM Role for Service Account
```bash
eksctl create iamserviceaccount \
    --name ebs-csi-controller-sa \
    --namespace kube-system \
    --cluster observability \ 
    --role-name AmazonEKS_EBS_CSI_DriverRole \
    --role-only \
    --attach-policy-arn arn:aws:iam::aws:policy/service-role/AmazonEBSCSIDriverPolicy \
    --approve
```

This command creates an IAM role for the EBS CSI controller. Because we will connect EBS and AWS, IAM role allows EBS CSI controller to interact with AWS resources, specifically for managing EBS volumes in the Kubernetes cluster.
We will attach the Role with service account

2. Retrieve IAM Role name
```bash
ARN=$(aws iam get-role --role-name AmazonEKS_EBS_CSI_DriverRole --query 'Role.Arn' --output text)
```
Command retrieves the ARN of the IAM role created for the EBS CSI controller service account.

3. Deploy EBS CSI 
```bash 
eksctl create addon --cluster observability --name aws-ebs-csi-driver --version latest \
    --service-account-role-arn $ARN --force
```
Above command deploys the AWS EBS CSI driver as an addon to your Kubernetes cluster.
It uses the previously created IAM service account role to allow the driver to manage EBS volumes securely.

4. Create Name Space for logging
```bash
kubectl create namespace logging
```

5. Install Elastic search on Kubernetes
```bash
helm repo add elastic https://helm.elastic.co

helm install elasticsearch \
 --set replicas=1 \
 --set volumeClaimTemplate.storageClassName=gp2 \ #this for ebs
 --set persistence.labels.enabled=true elastic/elasticsearch -n logging
```
Installs Elasticsearch in the logging namespace.
It sets the number of replicas, specifies the storage class, and enables persistence labels to ensure data is stored on persistent volumes.

6. Get username & password for Elastic search

```bash
# Username
kubectl get secrets --namespace=logging elasticsearch-master-credentials -ojsonpath='{.data.username}' | base64 -d

# Password
kubectl get secrets --namespace=logging elasticsearch-master-credentials -ojsonpath='{.data.password}' | base64 -d
```

7. Install Kibana
```bash
helm install kibana --set service.type=LoadBalancer elastic/kibana -n logging
```
Kibana provides a user-friendly interface for exploring and visualizing data stored in Elasticsearch.
It is exposed as a LoadBalancer service, making it accessible from outside the cluster.

8. Install Fluentbit with Custom Values/Configurations

```bash
helm repo add fluent https://fluent.github.io/helm-charts
helm install fluent-bit fluent/fluent-bit -f fluentbit-values.yaml -n logging
```

Before you Install this command you need to change default password from **``fluentbit-values.yaml``** with password from Elastic search (no 6) | (i.e NJyO47UqeYBsoaEU)

# Clean Up
```bash
helm uninstall monitoring -n monitoring

helm uninstall fluent-bit -n logging

helm uninstall elasticsearch -n logging

helm uninstall kibana -n logging

cd day-4

kubectl delete -k kubernetes-manifest/

kubectl delete -k alerts-alertmanager-servicemonitor-manifest/


eksctl delete cluster --name observability
```