# Data Pipeline in Kubernetes

Project for *SE-0000X: Cloud Computing Techonology*.  

# Project Overview
## Specs
#### This project is intended to do the following:
 * Receives JSON data from IOT devices,
 * Filters received data, 
 * Stores data into a distributed database.

#### To implement this behavior, this project uses:
 * TCP Socket to receive data from client,
 * Kafka for piping and filtering, (required by the course)
 * Redis as persistent cache, (required by the course)
 * Hadoop HBase for data storage. (required by thr course)

**Pipeline:**  Kafka -> Redis -> HBase

#### Development and Deployment
 * Pipeline connectors are developed in Java, and are dockernized,
 * All services are deployed in Kubernetes,
 * Kubernetes configs are templated using Helm.

## Project Structure
 * [ccproject](https://github.com/chenseanxy/1903-CCT-connector): Pipeline connectors in Java
 * [helm-charts](https://github.com/chenseanxy/1903-CCT-helm-charts): Helm charts used to deploy the services
 * [python-client](https://github.com/chenseanxy/1903-CCT-python-client): Simple python client used for testing purposes

# Getting Started
#### Cloning Project
```batch
git clone --recursive https://github.com/chenseanxy/1903-CCT-data-pipeline.git
```

#### Local Environment
To deploy, please make sure you have:
 * `docker`: For building `ccproject` image.
 * `kubectl`: For connecting to Kubernetes.
 * `helm`: For rendering Kubernetes config.

#### Deploying to Kubernetes
 * Install Helm in Kubernetes cluster
    ```batch
    kubectl --namespace kube-system create sa tiller
    kubectl create clusterrolebinding tiller --clusterrole cluster-admin --serviceaccount=kube-system:tiller
    helm init --service-account tiller
    ```

 * Deploy required services and frameworks
    ```batch
    helm install --name kfk ./kafka
    helm install --name redis ./redis
    helm install --name hbase-zk ./zookeeper
    helm install --name hbase ./hbase
    ```
 * Build `ccproject` image
 * Deploy pipeline connectors
    ```batch
    helm install --name ccproject ./ccproject --set image=<image-name>
    ```

All configurations are inside `helm-charts`, `values.yaml`.
