# Implementation of Traces
There are 2 part to Implement Traces
# 1. Developer
A developer of the application they have to do instrumentation In their application simmilarly how they instrument the ``Metrics``, and ``Logs``. 
There are multiple languages each programming language has some module, plugin, library. but prefer standart is to use ``open telemetry``. 

Open telemetry is vendor netral, let's say tommorrow if you want to move from one tracing tools to other it would be easy, but there are multiple standart, multiple module which can help you to write a tracing in your application. 

tracing can be more that is not only from service to service but you can also define instrumentation or tracing at the ``function level``, so you can understand from which function of a service the application route to another function, from there it goes to another service. and This part belongs to developer.

you can see some example of instrumentation the tracing in ``part-04 folder``.

# 2. Devops / SRE
Devops engineer they set up a kubernetes cluster, they set up the required configuration and Install and configure ``Jaegger`` or other traces tools. 

Only both of these things are done. you can implement distributed tracing, If one of these things not there the tracing won't distribute properly. let's say you are Devops engineer you have created kubernetes cluster, you have installed Jaegger using Helm, and you have configured other configuration. and The Developer of your organization did not Instrument the traces and eventually there is no point or you will have minimalistic. 

# Implementation
## 1. Instrumenting your Services
To start tracing, you need to instrument your services. This means adding tracing capabilities to your code. Most popular programming languages and frameworks have libraries or middleware that make this easy.
We have already instrumented our code using OpenTelemetry libraries/packages. For more details, refer to day-4/application/service-a/tracing.js or day-4/application/service-b/tracing.js.

## 2. Export Elastic search CA Certificate
This command retrieves the CA certificate from the Elasticsearch master certificate secret and decodes it, saving it to a ca-cert.pem file.
```bash
kubectl get secret elasticsearch-master-certs -n logging -o jsonpath='{.data.ca\.crt}' | base64 --decode > ca-cert.pem
```

## 3. Create tracing namespace
Creates a new Kubernetes namespace called tracing if it doesn't already exist, where Jaeger components will be installed.
```bash
kubectl create ns tracing
```

## 4. Create ConigMap for TLS's certificate
Creates a ConfigMap in the tracing namespace, containing the CA certificate to be used by Jaeger for TLS.
```bash
kubectl create configmap jaeger-tls --from-file=ca-cert.pem -n tracing
```

## 5. Create Secret for Elasticsearch TLS
Creates a Kubernetes Secret in the tracing namespace, containing the CA certificate for Elasticsearch TLS communication.
```bash
kubectl create secret generic es-tls-secret --from-file=ca-cert.pem -n tracing
```

## 6. Add jaeger helm repository
adds the official Jaeger Helm chart repository to your Helm setup, making it available for installations.
```bash
helm repo add jaegertracing https://jaegertracing.github.io/helm-charts

helm repo update
```

## 7. Install Jaeger with Custom Values
👉 Note: Please update the password field and other related field in the jaeger-values.yaml file with the password retrieved earlier in day-4 at step 6: (i.e NJyO47UqeYBsoaEU)"
Command installs Jaeger into the tracing namespace using a custom jaeger-values.yaml configuration file. Ensure the password is updated in the file before installation.

```bash
helm install jaeger jaegertracing/jaeger -n tracing --values jaeger-values.yaml
```

## 8. Port Forward Jaeger Query Service
Command forwards port 8080 on your local machine to the Jaeger Query service, allowing you to access the Jaeger UI locally.
```bash
kubectl port-forward svc/jaeger-query 8080:80 -n tracing
```

Using this command If you are using EC2 
```bash
kubectl port-forward svc/jaeger-query 8080:80 -n tracing
```

## 9. Clean up 

```bash

helm uninstall jaeger -n tracing

helm uninstall elasticsearch -n logging

# Also delete PVC created for elasticsearch

helm uninstall monitoring -n monitoring

cd day-4

kubectl delete -k kubernetes-manifest/

kubectl delete -k alerts-alertmanager-servicemonitor-manifest/

# Delete cluster
eksctl delete cluster --name observability

```