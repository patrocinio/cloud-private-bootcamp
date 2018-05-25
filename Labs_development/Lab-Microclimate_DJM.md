Lab - Microclimate
---

### Table of contents
[1. Install Microclimate](#install)

[2. Deploy the NodeJS Helm Chart using the ICP Console](#consoleDeploy)

[3. Deploy the NodeJS Helm Chart using the Helm CLI](#cmdDeploy)

## Overview
In this lab exercise you will install **microclimate** in to your ICP Cluster and then work through developing applications from scratch and importing existing applications

### Install Microclimate <a name="install"></a>
In this section you will install Microclimate using the IBM provided Helm Chart

#### Create secret and patch service account

1. In a **terminal** session connected to your `master` node as the **root** user issue the following command to install the `kubectl` Kubernetes CLICreate a Docker registry secret in the default namespace:

   ```
   kubectl create secret docker-registry microclimate-registry-secret \
     --docker-server=mycluster.icp:8500 \
     --docker-username=admin \
     --docker-password=admin \
     --docker-email=null
   ```



2. Patch the service account

    ```
    kubectl patch serviceaccount default -p '{"imagePullSecrets": [{"name": "microclimate-registry-secret"}]}'
    ```

#### Create Persistant Volumes (PV) and Persistant Volume Claims (PVC)

Microclimate requires two PVC to function one to store workspace data and another for Jenkins. The follwing steps will walk you through the process of creating the PV and PVC.

**Note:** In this lab we are using hostPath storage. This should only be used for testing and should not be used in a production environment.

1. Create the directories that will be mapped to the PV

    ```
    mkdir /mc-workspace

    mkdir /mc-jenkins
    ```

2. Create the required Persistant Volumes
   - Copy the following PV definition it a file named **mc-worspace-pv.yaml** on the system where you sourced your kubectl environment.

    ```
    apiVersion: v1
    kind: PersistentVolume
    metadata:
      name: microclimate-workspace
    spec:
      accessModes:
        - ReadWriteOnce
      persistentVolumeReclaimPolicy: Retain
      capacity:
        storage: 2Gi
      hostPath:
        path: /mc-workspace
    ```

   - Copy the following PV definition it a file named **mc-jenkins-pv.yaml** on the system where you sourced your kubectl environment.

    ```
    apiVersion: v1
    kind: PersistentVolume
    metadata:
      name: microclimate-jenkins
    spec:
      accessModes:
        - ReadWriteOnce
      persistentVolumeReclaimPolicy: Retain
      capacity:
        storage: 8Gi
      hostPath:
        path: /mc-jenkins
    ```



3. Create Persistant Volume Claims

   - Copy the following PV definition it a file named **mc-worspace-pvc.yaml** on the system where you sourced your kubectl environment.

    ```
   kind: PersistentVolumeClaim
   apiVersion: v1
   metadata:
     name: mc-workspace-pvc
   spec:
     accessModes:
       - ReadWriteOnce
     resources:
       requests:
         storage: 2Gi
    ```

   - Copy the following PV definition it a file named **mc-jenkins-pvc.yaml** on the system where you sourced your kubectl environment.

    ```
   kind: PersistentVolumeClaim
   apiVersion: v1
   metadata:
     name: mc-jenkins-pvc
   spec:
     accessModes:
       - ReadWriteOnce
     resources:
       requests:
         storage: 8Gi
    ```

4. Create the Persistant Volumes and Persistant Volume Claims.

   ```
   kubectl create -f ./mc-worspace-pv.yaml

   kubectl create -f ./mc-jenkins-pv.yaml

   kubectl create -f ./mc-worspace-pvc.yaml

   kubectl create -f ./mc-jenkins-pvc.yaml
   ```



5. Verify that the PVC have successfuly **Bound** to the PV.

   Execute: kubectl get pvc -n default

```
root@trona1:~# kubectl get pvc -n default
NAME               STATUS    VOLUME                 CAPACITY   ACCESS MODES   STORAGECLASS   AGE
mc-jenkins-pvc     Bound     microclimate-jenkins   8Gi        RWO                      1m
mc-workspace-pvc   Bound     mc-workspace           2Gi        RWO                     23s
```



#### Deploy Microclimate Helm Chart

1. Navigate to the ICP Catalog **Catalog -> Helm Charts** and search for the microclimate chart.

2. Click on the chart, read the overview and then click **Configure.**

3. Modify the following fields

   <u>Under the Configuration Section</u>

   ​	**Release name:** microclimate

   ​	**Target namespace:** default (Note: Must be default)

   ​	Accept the the license agreement

   <u>Under the Persistence Section</u>

   ​	**Existing PersistentVolumeClaim Name:** mc-workspace-pvc

   ​	De-select  Dynamic Provisioning

   <u>Under Jenkins Configuration Section</u>

   ​  **Jenkins hostname:   jenkins.Team_IP_Address.nip.io

   ​	**Jenkins - Existing PersistentVolumeClaim Name:** mc-jenkins-pvc

   **Note:** In a standard ICP topology the Jenkins hostname should be the IP of the Proxy node and in an HA topology you would specify the Proxy node VIP.

4. Click **Install**

5. You can monitor the status of your deployment from the CLI. May take up to 5 minutes.

   Execute: kubectl get po

```
root@trona1:~# kubectl get po

NAME                                                    READY     STATUS    RESTARTS   AGE
ldap-64745886dd-p6lq9                                   1/1       Running   0          3h
microclimate-ibm-microclimate-64cf96cc75-tlm6r          3/3       Running   0          6m
microclimate-ibm-microclimate-devops-86db55bd57-v76hd   1/1       Running   0          6m
microclimate-jenkins-56766f9b49-slxtw                   1/1       Running   0          6m
tiller-deploy-6c64d9f9c6-lljrh                          1/1       Running   0          1m
```



6. Once all the pods are running you can determine how microservice can be accessed.

   Execute the commands below to determine the Microclimate URL.

```
  export PORTAL_NODE_PORT=$(kubectl get svc microclimate-ibm-microclimate -o 'jsonpath={.spec.ports[?(@.name=="portal-http")].nodePort}')
  export NODE_IP=$(kubectl get nodes --namespace default -o jsonpath="{.items[0].status.addresses[0].address}")
  echo http://$NODE_IP:$PORTAL_NODE_PORT
```

​	Example output

```
root@yucca1:~# export PORTAL_NODE_PORT=$(kubectl get svc microclimate-ibm-microclimate -o 'jsonpath={.spec.ports[?(@.name=="portal-http")].nodePort}')
root@yucca1:~#   export NODE_IP=$(kubectl get nodes --namespace default -o jsonpath="{.items[0].status.addresses[0].address}")
root@yucca1:~#   echo http://$NODE_IP:$PORTAL_NODE_PORT
http://9.30.51.175:31341

```

#### Importing a project into Microclimate

1. Open the URL from the output of the commands in your browser
2. Read and accept the Microclimate license agreement and click **Accept**.

3. Select Import Project
4. Enter "https://github.com/microclimate-demo/node" in Git field and click **Next**.
5. Verify the Authentication required is not selected and click **Import**.



#### Creating a Microclimate pipeline

1. Once the workspace loads Select the **Pipeline** view.

2. Click **Create pipeline**

3. Enter https://github.com/microclimate-demo/node in the Repository location field.

4. Click **Create pipeline**.

5. You should now see the message "The pipeline has been configured for this project". Click **Open pipeline**.

6. The Jenkins interface should open and you should see a build in progress. Select the **Master** depolyment.

7. Monitor the Stage view until the build is complete. There will be three stages Extract, Docker build and Deploy. This make take up to 5 minutes to complete.

8. Once the build process has completed. Open the ICP interface:

   https://*Team_IP_Address*:8443

#### Verify the application deployment

1. Once logged into the ICP interface navigate to **Network Access -> Services**.
2. Search for *node* in the search bar and select the **node-service** in the **microclimate-pipeline-deployments** namespace.
3. Click on the link next to the Node port field to open up the associated application.
4. You should see the "*Hello world! This is a StarterKit!*" message confirming the application is running.



## Extra credit #1

1. Clone the project from the sample library and create your own Github repo.
2. Modify the message in the /node/public/index.html file and commit the changes to your new repository.
3. Import the project into Microclimate and create a new pipeline.
4. Monitor the build process verify your new message shows up when you access the application from ICP.
5. Change the index.html message again and commit your changes.
6. Monitor your Jenkins pipeline. What happens?



## Extra credit #2

1. Pick another project from the samples available.

   https://github.com/microclimate-demo

2. Import one of the other projects.

3. Create a build pipeline.

4. Monitor the build process.

5. What happens?




















## End of Lab Exercise
