+++
date = "2019-02-12T17:09:14-05:00"
draft = false
title = "Container Storage Interface"
slug = 'storage-k8s'

+++

CSI is the container storage interface. It is a plugin for Kubernetes and other container orchestrators that allows storage suppliers to expose their products to containerised applications as persistent storage. You can look the detailed specification [here](https://github.com/container-storage-interface/spec). 

Although prior to CSI Kubernetes provided a powerful volume plugin system, it was challenging to add support for new volume plugins to Kubernetes: volume plugins were “in-tree” meaning their code was part of the core Kubernetes code and shipped with the core Kubernetes binaries—vendors wanting to add support for their storage system to Kubernetes (or even fix a bug in an existing volume plugin) were forced to align with the Kubernetes release process.

CSI was developed as a standard for exposing arbitrary block and file storage storage systems to containerized workloads on Container Orchestration Systems (COs) like Kubernetes. Using CSI, third-party storage providers can write and deploy plugins exposing new storage systems in Kubernetes without ever having to touch the core Kubernetes code.