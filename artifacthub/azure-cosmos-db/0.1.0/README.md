# KEDA External Scaler for Azure Cosmos DB

The KEDA external scaler for Azure Cosmos DB enables users to scale their Kubernetes application based on changes to [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) container data. Version 0.1.0 is the first beta version of the external scaler.

## Overview

KEDA is a Kubernetes-based Event Driven Autoscaler. It enables auto-scaling of applications based on several external events. Check [KEDA documentation](https://keda.sh/docs/concepts/) to learn more.

While KEDA ships with a set of built-in scalers, it also allows users to extend KEDA through support for [external scalers](https://keda.sh/docs/scalers/external/). In this scheme, KEDA will query user's GRPC service to fetch metrics of an event source and will scale the applications accordingly. This is where 'KEDA external scaler for Azure Cosmos DB' plugs itself in. For information on how an external scaler can be implemented, check [KEDA external scaler concept](https://keda.sh/docs/concepts/external-scalers/).

## Setup Instructions

### Deploy KEDA and External Scaler

1. Add and update Helm chart repo.

    ```shell
    helm repo add kedacore https://kedacore.github.io/charts
    helm repo update
    ```

1. Install KEDA Helm chart (*or follow one of the other installation methods on [KEDA documentation](https://keda.sh/docs/deploy)*).

    ```shell
    helm install keda kedacore/keda --namespace keda --create-namespace
    ```

1. Install 'Cosmos DB scaler' Helm chart.

    ```shell
    helm install cosmosdb-scaler kedacore/keda-external-scaler-azure-cosmos-db --namespace keda --create-namespace
    ```

### Create `ScaledObject` Resource

Create `ScaledObject` resource that contains the information about your application (the scale target), the external scaler service, Cosmos DB containers, and other scaling configuration values. Check [`ScaledObject` specification](https://keda.sh/docs/concepts/scaling-deployments/) and [`External` trigger specification](https://keda.sh/docs/scalers/external/) for information on different properties supported for `ScaledObject` and their allowed values.

## Trigger Specification

The specification below describes the `trigger` metadata in `ScaledObject` resource for using 'KEDA external scaler for Cosmos DB' to scale your application.

```yaml
  triggers:
    - type: external
      metadata:
        scalerAddress: keda-external-scaler-azure-cosmos-db.keda:4050 # Mandatory. Address of the external scaler service.
        connection: <connection>               # Mandatory. Connection string of Cosmos DB account with monitored container.
        databaseId: <database-id>              # Mandatory. ID of Cosmos DB database containing monitored container.
        containerId: <container-id>            # Mandatory. ID of monitored container.
        leaseConnection: <lease-connection>    # Mandatory. Connection string of Cosmos DB account with lease container.
        leaseDatabaseId: <lease-database-id>   # Mandatory. ID of Cosmos DB database containing lease container.
        leaseContainerId: <lease-container-id> # Mandatory. ID of lease container.
        processorName: <processor-name>        # Mandatory. Name of change-feed processor used by listener application.
```

## Further Reading

- [Documentation](https://github.com/kedacore/keda-external-scaler-azure-cosmos-db#readme)
- [Release Notes](https://github.com/kedacore/keda-external-scaler-azure-cosmos-db/releases/tag/v0.1.0)
- [Example Usage](https://github.com/kedacore/keda-external-scaler-azure-cosmos-db/tree/main/src/Scaler.Demo)
