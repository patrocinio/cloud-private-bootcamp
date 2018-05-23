Lab - Install CLI and Tools
---

### Table of contents
[1. Install kubcetl](#kubectl)

[2. Configure kubcetl to connect to your ICP Cluster](#connect)

## Overview
In this lab exercise you will install the IBM Cloud Private CLI and other useful tools.

### Install kubectl <a name="kubectl"></a>
1. In a **terminal** session connected to your `master` node as the **root** user issue the following command to install the `kubectl` Kubernetes CLI

```
docker run -e LICENSE=accept --net=host -v /usr/local/bin:/data ibmcom/icp-inception:2.1.0.2-ee cp /usr/local/bin/kubectl /data
```

### Configure kubectl to connect to your ICP Cluster <a name="connect"></a>
1. If you aren't already logged in to the ICP Admin Console from a previous exercise, open a browser and navigate to `https://<icp_master_ip>/8443` and log in using `username: admin` and `password: admin`

2. Click the **User** icon on the navigation bar and then select **Configure Client** to display the commands that are used to configure a kubectl command line to connect to this ICP Cluster.

![Configure Client](images/kubectl/configureclient.jpg)

3. When the **Configure client** dialog is displayed, click the copy commands icon as shown below:

![Copy Commands](images/kubectl/copycommands.jpg)

4. Return to

### Getting Started <a name="gettingstarted"></a>
The first screen that is displayed when you successfully log in is the **Getting Started** page.
![ICP Getting Started Screen](images/treasurehunt/getstarted.jpg)

Locate the following information:

1. Which catalog item would you use if you want to **migrate an application that uses WebSphere Application Server**?

2. Which catalog item would you use if you want to **build a 12-factor microservice**?

3. What tool could you use to **chat with the team** if you have any issues?


## End of Lab Exercise
