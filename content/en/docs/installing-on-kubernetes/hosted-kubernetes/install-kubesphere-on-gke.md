---
title: "Deploy KubeSphere on GKE"
keywords: 'Kubernetes, KubeSphere, GKE, Installation'
description: 'Learn how to deploy KubeSphere on Google Kubernetes Engine.'

weight: 4240
---

![KubeSphere+GKE](https://pek3b.qingstor.com/kubesphere-docs/png/20191123145223.png)

This guide walks you through the steps of deploying KubeSphere on [Google Kubernetes Engine](https://cloud.google.com/kubernetes-engine/).

## Prepare a GKE Cluster

- A standard Kubernetes cluster in GKE is a prerequisite of installing KubeSphere. Go to the navigation menu and refer to the image below to create a cluster.

  ![create-cluster-gke](https://ap3.qingstor.com/kubesphere-website/docs/create-cluster-gke.jpg)

- In **Cluster basics**, select a Master version. The static version `1.15.12-gke.2` is used here as an example.

  ![select-master-version](https://ap3.qingstor.com/kubesphere-website/docs/master-version.png)

- In **default-pool** under **Node Pools**, define 3 nodes in this cluster.

  ![node-number](https://ap3.qingstor.com/kubesphere-website/docs/node-number.png)

- Go to **Nodes**, select the image type and set the Machine Configuration as below. When you finish, click **Create**.

  ![machine-config](https://ap3.qingstor.com/kubesphere-website/docs/machine-configuration.jpg)

{{< notice note >}}

- To install KubeSphere 3.2.1 on Kubernetes, your Kubernetes version must be v1.19.x, v1.20.x, v1.21.x, or v1.22.x (experimental).
- 3 nodes are included in this example. You can add more nodes based on your own needs especially in a production environment.
- The machine type e2-medium (2 vCPU, 4GB memory) is for minimal installation. If you want to enable pluggable components or use the cluster for production, please select a machine type with more resources.
- For other settings, you can change them as well based on your own needs or use the default value.

{{</ notice >}}

- When the GKE cluster is ready, you can connect to the cluster with Cloud Shell.

  ![cloud-shell-gke](https://ap3.qingstor.com/kubesphere-website/docs/cloud-shell.png)

## Install KubeSphere on GKE

- Install KubeSphere using kubectl. The following commands are only for the default minimal installation.

  ```bash
  kubectl apply -f https://github.com/kubesphere/ks-installer/releases/download/v3.2.1/kubesphere-installer.yaml

  kubectl apply -f https://github.com/kubesphere/ks-installer/releases/download/v3.2.1/cluster-configuration.yaml
  ```

- Inspect the logs of installation:

  ```bash
  kubectl logs -n kubesphere-system $(kubectl get pod -n kubesphere-system -l app=ks-install -o jsonpath='{.items[0].metadata.name}') -f
  ```

- When the installation finishes, you can see the following message:

  ```yaml
  #####################################################
  ###              Welcome to KubeSphere!           ###
  #####################################################
  Console: http://10.128.0.44:30880
  Account: admin
  Password: P@88w0rd
  NOTES：
    1. After logging into the console, please check the
      monitoring status of service components in
      the "Cluster Management". If any service is not
      ready, please wait patiently until all components
      are ready.
    2. Please modify the default password after login.
  #####################################################
  https://kubesphere.io             2020-xx-xx xx:xx:xx
  ```

## Access KubeSphere Console

Now that KubeSphere is installed, you can access the web console of KubeSphere by following the steps below.

- In **Services & Ingress**, select the service **ks-console**.

  ![ks-console](https://ap3.qingstor.com/kubesphere-website/docs/console-service.jpg)

- In **Service details**, click **Edit** and change the type from `NodePort` to `LoadBalancer`. Save the file when you finish.

  ![lb-change](https://ap3.qingstor.com/kubesphere-website/docs/lb-change.jpg)

- Access the web console of KubeSphere using the endpoint generated by GKE.

  ![access-console](https://ap3.qingstor.com/kubesphere-website/docs/access-console.png)

  {{< notice tip >}}

  Instead of changing the service type to `LoadBalancer`, you can also access KubeSphere console via `NodeIP:NodePort` (service type set to `NodePort`). You may need to open port `30880` in firewall rules.

  {{</ notice >}}

- Log in to the console with the default account and password (`admin/P@88w0rd`). In the cluster overview page, you can see the dashboard.

## Enable Pluggable Components (Optional)

The example above demonstrates the process of a default minimal installation. To enable other components in KubeSphere, see [Enable Pluggable Components](../../../pluggable-components/) for more details.