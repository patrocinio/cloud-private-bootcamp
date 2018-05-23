# ICP Kubernetes and Network

## Overview

We sill start this session by introducing both Kubernetes and ICP networking elements with a strong emphasis on the Calico netowrk overlay.  We will draw upon real customer scenarios to discuss network segmentation and securing the cluster from the network perspective.  We will cover different methods of Ingress configuration and the ability to control egress.

### Kubernetes Networking High-Level

### Calico Networking High-Level

### Pod Network

### Service

### Endpoint

### Node Port versus Ingress Controller

### Kube-DNS

### Proxy Node

### Alternatives to the Proxy Node

### Creating a Segmented Network using Calico Policy (including egress)

### Kube-proxy and how it works

### Iptables versus ipvs

### Digging in at the system level with routes, iptables, etc.

### Network at Installation Time

### Troubleshooting

# Network Lab

If we have time I have laid out a lab that we could probably accomplish no matter the lab environment.

### Using Calico

### Calctl

### Create a namespace

### Disable all traffic in and out

### Enable specific traffic in

### Enable traffic out

### Check under the covers
