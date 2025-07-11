# Prometheus-Grafana-EKS

## 📊 Monitoring EKS Cluster with Prometheus & Grafana

This project sets up **Prometheus** and **Grafana** on an **AWS EKS** cluster to monitor Kubernetes workloads, nodes, and infrastructure metrics using Helm.

## 🚀 Creating an EKS Cluster Using YAML

You can create an EKS cluster using a Kubernetes manifest and `eksctl`. Below is an example `eks-cluster.yaml` configuration:

```yaml
apiVersion: eksctl.io/v1alpha5
kind: ClusterConfig

metadata:
  name: observability
  region: us-east-1

availabilityZones:
  - us-east-1a
  - us-east-1b

iam:
  serviceRoleARN: arn:aws:iam::975050354644:role/eksClusterRole

nodeGroups:
  - name: observability-ng-private
    instanceType: t3.medium
    desiredCapacity: 2
    minSize: 2
    maxSize: 3
    volumeSize: 20
    privateNetworking: true  # Equivalent to --node-private-networking
    iam:
      withAddonPolicies:
        autoScaler: true
        externalDNS: true
        appMesh: true
        albIngress: true
    asgMetricsCollection:
      - granularity: 1Minute

```

To create the cluster, run:

```sh
eksctl create cluster -f eks-cluster.yaml
```

This will provision an EKS cluster and a managed node group as specified in the YAML file.

