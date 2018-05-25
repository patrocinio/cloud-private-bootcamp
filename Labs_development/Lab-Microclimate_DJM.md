Lab - Microclimate
---

### Table of contents
[1. Configure ICP for Microclimate](#configure)

[2. Deploy the Microclimate Helm Chart](#deploychart)

[3. Deploy the NodeJS Helm Chart using the Helm CLI](#cmdDeploy)

## Overview
In this lab exercise you will install **microclimate** in to your ICP Cluster and then work through developing applications from scratch and importing existing applications

### Configure ICP for Microclimate <a name="configure"></a>
In this section you will configure the environment for Microclimate

#### Create secret and patch service account

1. In a **terminal** session connected to your `master` node as the **root** user issue the following command to create a Docker registry secret in the default namespace:

   ```
   kubectl create secret docker-registry microclimate-registry-secret \
     --docker-server=mycluster.icp:8500 \
     --docker-username=admin \
     --docker-password=admin \
     --docker-email=null
   ```

2. Issue the following command to patch the service account

    ```
    kubectl patch serviceaccount default -p '{"imagePullSecrets": [{"name": "microclimate-registry-secret"}]}'
    ```

#### Create Persistant Volumes (PV) and Persistant Volume Claims (PVC)
Microclimate requires two PVCs to function; one to store workspace data and another for Jenkins. The following steps will walk you through the process of creating the PV and PVC.

**Note**: In this lab environment, the NFS Server is running on the icp-proxy node. In a *real* environment a dedicated NFS Server will probably exist.

1. In a **terminal** session connected to your `proxy` node as the **root** user issue the following command to create the directories that will be mapped to the PV

    ```
    cd /storage

    mkdir mc-workspace

    mkdir mc-jenkins

    chmod 777 mc*
    ```

**Note:** These are the only commands you will enter on the `proxy` node in this exercise. All subsequent commands will be entered on the `master` node.

2. In a **terminal** session connected to your `master` node as the **root** user, copy the following PV definition in to a file named `mc-worspace-pv.yaml` and change the **server IP address** (9.37.138.12) to the correct one for your environment.

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
      nfs:
        path: /storage/mc-workspace
        server: 9.37.138.12
    ```

3. Copy the following PV definition in to a file named `mc-jenkins-pv.yaml` and change the **server IP address** (9.37.138.12) to the correct one for your environment.

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
      nfs:
        path: /storage/mc-jenkins
        server: 9.37.138.12
    ```

4. Copy the following PVC definition in to a a file named `mc-worspace-pvc.yaml`

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

5. Copy the following PVC definition in to a file named `mc-jenkins-pvc.yaml`

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

4. Create the Persistent Volumes and Persistent Volume Claims using the following commands:

   ```
   kubectl create -f ./mc-workspace-pv.yaml

   kubectl create -f ./mc-jenkins-pv.yaml

   kubectl create -f ./mc-worspace-pvc.yaml

   kubectl create -f ./mc-jenkins-pvc.yaml
   ```

5. Verify that the PVC have successfuly **Bound** to the PV by issuing the following command:

`kubectl get pvc -n default`

```
# kubectl get pvc -n default
NAME               STATUS    VOLUME                 CAPACITY   ACCESS MODES   STORAGECLASS   AGE
mc-jenkins-pvc     Bound     microclimate-jenkins   8Gi        RWO                      1m
mc-workspace-pvc   Bound     mc-workspace           2Gi        RWO                     23s
```

### Deploy the Microclimate Helm Chart <a name="deploychart"></a>
In this section you will deploy the Microclimate Helm Chart using the IBM Admin console

1. Click **Catalog** from the ICP Admin Console menu bar to navigate to the Catalog of Helm Charts and search for the `ibm-microclimate` chart.

2. Click on the chart, read the overview and then click **Configure.**

3. Enter the following information (accept the defaults for all other values) and click **Install**:

**NOTE**: You must change the Jenkins hostname value to include your icp-proxy-ip address for instance: jenkins.9.37.138.12.nip.io

  | Parameter       | Value |
  | ------------- |-------------|
  | Release name  | microclimate |
  | Target namespace  | default (Note: must be default) |
  | I have read and agreed to the License Agreements | yes |

  In the **Persistence** section:

  | Parameter       | Value |
  | ------------- |-------------|
  | Existing PersistentVolumeClaim Name | mc-workspace-pvc |
  | Dynamic Provisioning | no |

  In the **Jenkins** section:

  | Parameter       | Value |
  | ------------- |-------------|
  | Jenkins hostname | jenkins.icp-proxy-ip.nip.io |
  | Jenkins - Existing PersistentVolumeClaim Name | mc-jenkins-pvc |

5. You can monitor the status of your deployment from the CLI. Issue the following command `kubectl get po`

Note: It may take up to 5 minutes to get a **Running** status for all of the microclimate pods

```
# kubectl get po

NAME                                                    READY     STATUS    RESTARTS   AGE
microclimate-ibm-microclimate-64cf96cc75-tlm6r          3/3       Running   0          6m
microclimate-ibm-microclimate-devops-86db55bd57-v76hd   1/1       Running   0          6m
microclimate-jenkins-56766f9b49-slxtw                   1/1       Running   0          6m
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
