Lab - Console Treasure Hunt
---

### Table of contents
[1. Log in to your ICP Admin Console ](#login)

[2. Getting Started](#gettingstarted)

[3. Dashboard](#dashboard)

[4. Nodes](#nodes)

[4. Namespaces](#namespaces)

[5. Helm Charts](#helmcharts)

[7. Monitoring](#monitoring)

[8. Alerts](#alerts)

[9. Deployments](#deployments)

[10. StatefulSets](#statefulsets)

[11. DaemonSets](#daemonsets)

[12. Services](#services)

[13. Ingress](#ingress)

[14. Command Line Parameters](#cmdline)

## Overview
In this lab exercise you will become familiar with the IBM Cloud Private Administration Console by completing a Treasure Hunt.

### Log in to your ICP Admin Console <a name="login"></a>
If you aren't already logged in to the ICP Admin Console from a previous exercise, open a browser and navigate to `https://<icp_master_ip>/8443`

![ICP Login Screen](images/treasurehunt/login.jpg)

Log in using `username: admin` and `password: admin`

### Getting Started <a name="gettingstarted"></a>
The first screen that is displayed when you successfully log in is the **Getting Started** page.
![ICP Getting Started Screen](images/treasurehunt/getstarted.jpg)

Locate the following information:

1. Which catalog item would you use if you want to **migrate an application that uses WebSphere Application Server**?

2. Which catalog item would you use if you want to **build a 12-factor microservice**?

3. What tool could you use to **chat with the team** if you have any issues?

### Dashboard <a name="dashboard"></a>
Click **Menu** (in the top left corner of the page) and then select **Dashboard** to navigate to the Dashboard page. The Dashboard page provides an overview of the current status of the ICP cluster.
![ICP Dashboard](images/treasurehunt/dashboard.jpg)

Locate the following information:

1. How many **Nodes** are in your ICP Cluster?

2. How much **Storage** is currently available in your ICP Cluster?

3. Are all of the **Deployments** in your ICP Cluster healthy?

### Nodes <a name="nodes"></a>
Click **Menu** and then select **Platform > Nodes** to navigate to the Nodes page. This page displays information about the nodes that are part of the ICP Cluster.
![ICP Nodes Screen](images/treasurehunt/nodes.jpg)

Note: Click the **command prompt** icon in the bottom right corner of the screen to see the command that a user would need to issue from the **Kubernetes CLI command prompt** to see the same information that has been displayed in the Administration Console.

![ICP CLI Commands](images/treasurehunt/cli.jpg)

Click on the **Name** of the node to *drill down* and see more information about a node.
![ICP Node Name](images/treasurehunt/nodename.jpg)

Locate the following information:

1. How many **Worker nodes** are in your cluster?

2. What is the **Architecture** of your master node?

3. How much **memory** does your proxy node have?

4. How many **CPUs** do each of your worker nodes have?

5. Which node is the **logging-elk-data-0** pod deployed to?

### Namespaces <a name="namespaces"></a>
Click **Menu** and then select **Manage > Namespaces** to navigate to the Namespaces page.

Users are assigned to organizational units called namespaces.
Namespaces are also known as tenants or accounts. In IBM Cloud Private, users are assigned to teams. You can assign multiple namespaces to a team. Users of a team are members of the team's namespaces.
![ICP Namespaces](images/treasurehunt/namespaces.jpg)

Locate the following information:

1. How many **namespaces** have been automatically created in your cluster during installation?

2. What **actions** are you able to take on namespaces?

### Helm Charts <a name="helmcharts"></a>
Click **Catalog** on the navigation bar to navigate to the Helm Chart Catalog page.
![Catalog Link](images/treasurehunt/cataloglink.jpg)

By using the Catalog, you can browse and install packages in your IBM Cloud Private cluster from Helm charts.

The Catalog displays Helm charts, which contain application packages that can run as Kubernetes services. The packages are stored in repositories. The Catalog in IBM Cloud Private contains connections to recommended repositories by default, but you can connect to other repositories. After you connect to a repository, you can access its charts from the Catalog. Application developers can also develop applications and publish them in the Catalog so that other users can easily access and install the applications.

![ICP Catalog](images/treasurehunt/catalog.jpg)

Note: Click on the Helm Chart name to view the readme file

Locate the following information:

1. What date was the **ibm-jenkins-dev** Helm chart released?

2. How many MQ servers does the **ibm-mq-advanced-dev** Helm chart deploy?

3. What type of server does the **ibm-swift-sample** Helm chart deploy the sample application on?

Click **Menu** and then select **Manage > Helm Repositories** to navigate to the list of configured Helm repositories page.

![ICP Helm Repositories](images/treasurehunt/helmrepo.jpg)

### Storage <a name="storage"></a>
Click **Menu** and then select **Platform > Storage** to navigate to the Storage page.

### Monitoring <a name="monitoring"></a>
Click **Menu** and then select **Platform > Monitoring** to navigate to the Monitoring page.

### Alerts <a name="alerts"></a>
Click **Menu** and then select **Platform > Alert** to navigate to the Alerts page.

### Deployments <a name="deployments"></a>
Click **Menu** and then select **Workloads > Deployments** to navigate to the Deployments page.

### StatefulSets <a name="statefulsets"></a>
Click **Menu** and then select **Workloads > StatefulSets** to navigate to the StatefulSets page

### DaemonSets <a name="daemonsets"></a>
Click **Menu** and then select **Workloads > DaemonSets** to navigate to the DaemonSets page

### Services <a name="services"></a>
Click **Menu** and then select **Network Access > Services** to navigate to the Services page.

### Ingress <a name="ingress"></a>
Click **Ingress** to navigate to the Ingress page.

#### Command Line Parameters <a name="cmdline"></a>
Click the **User** icon on the navigation bar and then select **Configure Client** to display the commands that are used to configure a kubectl command line to connect to this ICP Cluster.
