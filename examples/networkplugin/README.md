# AKS Engine - Network Plugin

There are 5 different Network Plugin options :

- Azure Container Networking (default)
- Kubenet
- Flannel (docs are //TODO)
- Cilium (CNI IPAM implementation that pairs w/ cilium NetworkPolicy addon; only works w/ `"networkPolicy": "cilium"`)
- Antrea (CNI IPAM implementation that pairs w/ antrea NetworkPolicy addon; only works w/ `"networkPolicy": "antrea"`)

## Azure Container Networking (default)

By default (currently Linux clusters only), the `azure` network policy is applied. It is an open source implementation of [the CNI Network Plugin interface](https://github.com/containernetworking/cni/blob/master/SPEC.md) and [the CNI Ipam plugin interface](https://github.com/containernetworking/cni/blob/master/SPEC.md#ip-address-management-ipam-interface)

CNI brings the containers to a single flat L3 Azure subnet. This enables full integration with other SDN features such as network security groups and VNET peering. The plugin creates a bridge for each underlying Azure VNET. The bridge functions in L2 mode and is connected to the host network interface.

If the container host VM has multiple network interfaces, the primary network interface is reserved for management traffic. A secondary interface is used for container traffic whenever possible.

More detailed documentation can be found in [the Azure Container Networking Repository](https://github.com/Azure/azure-container-networking/tree/master/docs)

Example of templates enabling Azure CNI:

```json
  "properties": {
    "orchestratorProfile": {
      "kubernetesConfig": {
        "networkPlugin": "azure"
      }
    }
    ...
  }
```

Or by not specifying any network plugin, you'll get Azure CNI by default:

```json
  "properties": {
    "orchestratorProfile": {
      "kubernetesConfig": {
        <other kubernetesConfig properties>
      }
    }
    ...
  }
```

## Kubenet

Also available is the Kubernetes-native kubenet implementation, which is declared as configuration thusly:

```json
  "properties": {
    "orchestratorProfile": {
      "kubernetesConfig": {
        "networkPlugin": "kubenet"
      }
    }
    ...
  }
```
