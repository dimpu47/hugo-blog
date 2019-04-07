+++
date = "2018-11-19T15:07:32+05:30"
title = "Horizontal Pod Autoscaler."
draft = false
slug = 'k8s-hpa'
+++

## Horizontal Pod Autoscaler - HPA
*HorizaontalPodAutoscaler* is a Kubernetes Resource that ***automatically scales*** *Pod Replicas* managed by a Controller. This automaic scaling is performed by *Horizontal Controllers*, which is enabled and configured by creating a HorizontalPodAutoscaler(HPA) Resource. 
The Controller periodically checks pod metrics, calculates the no. of replicas required to meet the target value configured in HPA resource and adjust/update replica field on target resource i.e. Deployment, ReplicaSet, StatefulSet, ReplicationController.

### Understanding how HorizontalPodAutoscaler works
Autoscaling is split into three steps:
     
- Obtain metrics of all the pods managed by scaled resource object.
- Calculate no. of pods required to bring the metrics to (or close to) the specified target value.
- Update the replica field of the scaled value.

let's get further into them..

#### obtaining pod metrics

