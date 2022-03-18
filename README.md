# Airflow + Spark on Kubernetes

Running Airflow with Kubernetes on Kubernetes(Minikube)

## Requirements

- [Docker](https://www.docker.com/)
- [Minikube](https://minikube.sigs.k8s.io/docs/start/)
- [Kubectl](https://kubernetes.io/docs/tasks/tools/#kubectl)
- [Helm](https://helm.sh/)

## Steps

### Adding Helm repositories

```bash
helm repo add apache-airflow https://airflow.apache.org
helm repo add spark-operator https://googlecloudplatform.github.io/spark-on-k8s-operator
```

### Creating Kubernetes on Miniube
```bash
minikube start --cpus 7 --memory 10000  --kubernetes-version=v1.22.0
```

### Configuring cluster

#### Labeling Nodes
```bash
kubectl label nodes minikube nodePool=cluster
```

#### Setting cluster admin
```bash
kubectl create clusterrolebinding permissive-binding --clusterrole=cluster-admin --user=admin --user=kubelet --group=system:serviceaccounts
```

#### Installing Spark Operator on cluster
```bash
helm install my-release spark-operator/spark-operator --version 1.1.5
```

### Installing Airflow on Kubernetes
```bash
kubectl create ns spark-jobs --dry-run=client -o yaml | kubectl apply -f -
helm install airflow apache-airflow/airflow --namespace spark-jobs  --timeout 600s
```