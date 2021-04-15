+++
date = "2020-02-02T17:09:14-05:00"
draft = false
title = "Extending Kubernetes"
slug = 'extending-kubernetes'

+++

### How would you extend k8s or why would you want to ?

Answering the why is easy but hard as well. Easy, because it allows you to create custom resources in k8s, reconciliation loop to control or manage those custom resources according to your own rules and expertise on your own custom resource. Hard, because now you'll have to think of usecases where this is suitable. For inspiration you can look at the [etcd operator](https://github.com/coreos/etcd-operator) by coreos. Also do check out more examples to suffice your added excitement at [operator hub](https://operatorhub.io/) which is were you can also publish your own custom operators and share it with the broader community! 

now, about the how? will take a bit longer and I will try to cover in as much detail as possible in this article.

Basically you will have done these things, by the end:

- Create a custom object in K8s.
- Create controller for custom object.
- Add custom API servers.
- Self provisioning of services with K8s service catalog.

These according to me are core concept to extend k8s API, although coreos had introduced a concept of [operators](/blog/k8s-operator) which is another way to automate packaging, deploying and managing k8s custom resources with domain expertise required for such a resource.

## Custom Resource Definition.
If you're remotely familiar with basic of k8s, You must have heard about k8s resources like pods, service, deployments, ingress etc. These are all native resource available in k8s out of the box, without any special configuration or installation of k8s. In the same way CRD (CustomResourceDefinition) is an object of k8s api, which allows k8s users to define your Custom Resource. Once you post this object to k8s api, you can then post JSON/YAML to create those Custom resources in a same way native k8s resources like pods, services, configmaps etc.

An example to define a custom resource:
```yaml
apiVersion: apiextensions.k8s.io/v1beta1
kind: CustomResourceDefinition
metadata:
  name: websites.extensions.example.com
spec:
  scope: Namespaced
  group: extensions.example.com
  version: v1
  names:
    # kubectl create website or kubectl create websites
    kind: Website
    singular: website
    plural: websites
```
after you post above descriptor to k8s api, you can create multiple custom reources of `kind: Website`. For which you could use the following manifest:
```yaml
kind: Website
metadata:
  name: kubia
spec:
  # git repo url where you static web content is stored. 
  gitRepo: <git-repo-url>
```

This custom resource is of no use without a controller. To make our Website objects run a web server pod exposed through a Service, you’ll need to build and deploy a Website controller, which will watch the API server for the creation of Website objects and then create the Service and the web server Pod for each of them.

To make sure the Pod is managed and survives node failures, the controller will create a Deployment resource instead of an unmanaged Pod directly. We will write a very simple controller that watches our website object on the k8s api with the help of `kubectl proxy` process a sidecar acting as Ambassador to the k8s API server, forwading the request to API server taking care of TLS encryptions and authentication.

API Server will send watch events for every changes to an Website Object.
