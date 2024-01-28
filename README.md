# Redis Chart

## Introduction

Welcome to the Redis Chat user guide for deploying Bitnami Redis and Redis Cluster using a single Helm Chart. This chart provides flexibility by allowing users to choose between deploying Bitnami Redis or Bitnami Redis Cluster based on their requirements.

> Note: The Redis Cluster chart will deploy a Redis Cluster topology with sharding while the Redis ordinary chart deploys either a standalone, replication, or sentinel-based cluster. For more information on the differences between Redis and Redis cluster chart, please refer to [this link](https://docs.bitnami.com/kubernetes/infrastructure/redis/get-started/compare-solutions/).


### Prerequisites

Before you begin, ensure that you have the following prerequisites installed:

- Helm: [Install Helm](https://helm.sh/docs/intro/install/)

## Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/snapp-incubator/redis-chart
cd redis-chart
```

### 2. Customize Values

Edit the `values.yaml` file to configure your deployment. Two main options control the Redis type:

- `redis.enabled`: Set to `true` to enable Bitnami Redis deployment in Standalone or Sentinel mode. For more information about the Helm chart, please refer to [documentation](https://github.com/bitnami/charts/tree/main/bitnami/redis).
- `redis-cluster.enabled`: Set to `true` to enable Bitnami Redis Cluster deployment. For more information about the Bitnami Redis Helm chart, please refer to [documentation](https://github.com/bitnami/charts/tree/main/bitnami/redis-cluster).

Example `values.yaml`:

```yaml
redis:
  enabled: true
  metrics:
    enabled: true
  sentinel: 
    enabled: true
    
redis-cluster:
  enabled: false
```

### 3. Deploy the Helm Chart

```bash
helm install my-redis-chart . -n <namespace>
```

Replace `my-redis-chart` with a name of your choice.

### 4. Verify Deployment

Check the status of the deployment:

```bash
helm status my-redis-chart -n <namespace>
```

## Configuration Options

### Bitnami Redis

For Bitnami Redis-specific configurations, refer to the Bitnami Redis documentation.

Example customization in `values.yaml`:

```yaml
redis:
  enabled: true
  # Add Bitnami Redis-specific configurations here. Please refer to Bitnami Redis repository for all of available values.
  commonConfiguration: |-
    # Enable AOF https://redis.io/topics/persistence#append-only-file
    append-only yes
    # Disable RDB persistence, AOF persistence is already enabled.
    save ""
  master:
    service:
      type: ClusterIP
```

### Bitnami Redis Cluster

For Bitnami Redis Cluster-specific configurations, refer to the Bitnami Redis Cluster.

Example customization in `values.yaml`:

```yaml
redis-cluster:
  enabled: true
  # Add Bitnami Redis Cluster-specific configurations here. Please refer to Bitnami Redis repository for all of available values. 
  cluster:
    init: true
    nodes: 6
    replicas: 1
```

> Caution: Before attempting to scale the Redis Cluster, refer to the Bitnami Redis Cluster documentation for detailed instructions and best practices. Incorrectly scaling the cluster can lead to data inconsistency and operational issues.

## Cleanup

To uninstall the deployed Helm Chart, use the following command:

```bash
helm uninstall my-redis-chart -n <namespace>
```
