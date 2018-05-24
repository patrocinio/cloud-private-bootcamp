Lab - Install CLI and Tools
---

### Table of contents
[1. Install kubectl](#kubectl)

[2. Configure kubectl to connect to your ICP Cluster](#connect)

[3. Install the ICP CLI](#bxcli)

[4. Install the Helm CLI](#helm)

## Overview
In this lab exercise you will install the Kubernetes CLI, the IBM Cloud Private CLI and other useful tools.

### Install kubectl <a name="kubectl"></a>
In a **terminal** session connected to your `master` node as the **root** user issue the following command to install the `kubectl` Kubernetes CLI

```
docker run -e LICENSE=accept --net=host -v /usr/local/bin:/data ibmcom/icp-inception:2.1.0.2-ee cp /usr/local/bin/kubectl /data
```

### Configure kubectl to connect to your ICP Cluster <a name="connect"></a>
If you aren't already logged in to the ICP Admin Console from a previous exercise, open a browser and navigate to `https://<icp_master_ip>/8443` and log in using `username: admin` and `password: admin`

Click the **User** icon on the navigation bar and then select **Configure Client** to display the commands that are used to configure a kubectl command line to connect to this ICP Cluster.

![Configure Client](images/kubectl/configureclient.jpg)

When the **Configure client** dialog is displayed, click the copy commands icon as shown below:

![Copy Commands](images/kubectl/copycommands.jpg)

Return to the terminal window and paste in the commands. The output will be similar to that shown below:

```
# kubectl config set-cluster cluster.local --server=https://9.37.138.189:8001 --insecure-skip-tls-verify=true
Cluster "cluster.local" set.

# kubectl config set-context cluster.local-context --cluster=cluster.local
Context "cluster.local-context" created.

# kubectl config set-credentials admin --token=...
User "admin" set.

# kubectl config set-context cluster.local-context --user=admin --namespace=default
Context "cluster.local-context" modified.

# kubectl config use-context cluster.local-context
Switched to context "cluster.local-context".
```

Issue the following command to get information about your ICP Cluster: `kubectl cluster-info`

```
# kubectl cluster-info
Kubernetes master is running at https://9.37.138.189:8001
catalog-ui is running at https://9.37.138.189:8001/api/v1/namespaces/kube-system/services/catalog-ui:catalog-ui/proxy
Heapster is running at https://9.37.138.189:8001/api/v1/namespaces/kube-system/services/heapster/proxy
image-manager is running at https://9.37.138.189:8001/api/v1/namespaces/kube-system/services/image-manager:image-manager/proxy
KubeDNS is running at https://9.37.138.189:8001/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
metrics-server is running at https://9.37.138.189:8001/api/v1/namespaces/kube-system/services/https:metrics-server:/proxy
platform-ui is running at https://9.37.138.189:8001/api/v1/namespaces/kube-system/services/platform-ui:platform-ui/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```

### Install the ICP CLI <a name="bxcli"></a>
In the terminal window, issue the following commands to download the **IBM Cloud CLI**

```
cd /tmp
wget https://clis.ng.bluemix.net/download/bluemix-cli/0.6.1/linux64
```

Issue the following commands to install the IBM Cloud CLI

```
mv ./linux64 bx.tar.gz
tar -xvf bx.tar.gz
cd Bluemix_CLI*
./install_bluemix_cli
```

Issue the following command to download the **ICP CLI** from your ICP instance (insert the IP address of your master node)

```
cd /tmp
wget "https://<icp_master_ip>:8443/api/cli/icp-linux-amd64" --no-check-certificate
```

Issue the following command to install the ICP CLI

```
bx plugin install ./icp-linux-amd64
```

Issue the following command to login the ICP CLI in to your ICP Cluster.  

```
bx pr login -a https://<icp_master_ip>:8443 --skip-ssl-validation
```

Enter `username: admin` and `password: admin` when prompted and select the `mycluster Account` as shown below

```
# bx pr login -a https://9.37.138.189:8443 --skip-ssl-validation
API endpoint: https://9.37.138.189:8443
Username> admin
Password>
Authenticating...
OK

Select an account:
1. mycluster Account (id-mycluster-account)
Enter a number> 1
Targeted account mycluster Account (id-mycluster-account)

Configuring helm and kubectl...
Configuring kubectl: /root/.bluemix/plugins/icp/clusters/mycluster/kube-config
Property "clusters.mycluster" unset.
Property "users.mycluster-user" unset.
Property "contexts.mycluster-context" unset.
Cluster "mycluster" set.
User "mycluster-user" set.
Context "mycluster-context" created.
Switched to context "mycluster-context".

Cluster mycluster configured successfully.

Configuring helm: /root/.helm
Helm configured successfully

OK
```

Issue the following command to get information about your cluster

```
bx pr cluster-get mycluster
```

The results of the command are shown below:
```
# bx pr cluster-get mycluster
Retrieving cluster mycluster...
OK

Name:			mycluster
ID:			00000000000000000000000000000001
State:			deployed
Master URL:		https://kubernetes.default
Masters:		1
Workers:		3
Proxies:		1
```

# wget https://$accessip:8443/helm-api/cli/linux-amd64/helm --no-check-certificate
# chmod 755 helm
# mv helm /usr/local/bin
# helm init -c

## End of Lab Exercise
