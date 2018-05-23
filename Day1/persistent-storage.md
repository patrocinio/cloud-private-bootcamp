# Persistent Storage and ICP K8s

## In this session we will cover persistent storage options and decisions.  We will provide information behind the pros and cons for each storage configuration.  Different platforms and customers present the opportunity for different solutions.  Attendees should be prepared to make these often loaded decisions at anyof thier customers.

### Persistent Storage Concepts (PV, PVC, Storage Class)

### Access and Reclaim Policy

### Storage Plugins (Beyond the Mainstream)

### Docker and Storage (Overlays and Orphans)

## Our Storage Providers

### HostPath

### NFS

### vSphereVolumes

### GlusterFS

### Ceph

### IBM Storage via FlexVolume

### Making Storage Decisions Based Upon the Correct Criteria

# Persistent Storage Lab

## Lab Overview
Note:  I am not certain whether or not I want to do this lab as GlusterFS and Heketi or if there is a better alternative.  This will depend on our lab environment.

### Configure a storage cluster (depends on our lab environment)

### Maybe create a GlusterFS

### Maybe create Heketi as a pod

### Maybe configure Heketi to backup its DB as a secret

### Maybe show Heketi using config map for its configuration

### Maybe define storage class

### Demonstrate using the storage class

### Altering a Helm chart to change from one type of storage to another
