+++
date = "2020-02-02T17:09:14-05:00"
draft = false
title = "Extending Kubernetes"
slug = 'extending-kubernetes'

+++

### How do you extend k8s or why would you want to ?

Answering the why is easy, because it allows you to create custom resources, reconciliation loop to control or manage it according to your own rules and expertise on your own custom resource.

the how? will take a bit longer and will be covered in this article.

Basically you would have done these things:

- Create a custom object in K8s.
- Create controller for custom object.
- Add custom API servers.
- Self provisioning of services with K8s service catalog.

These according to me are core concept to extend k8s API, although coreos had introduced a concept of [operators](/blog/k8s-operator) which is another way to automate packaging, deploying and managing k8s custom resources with domain expertise required for such a resource.