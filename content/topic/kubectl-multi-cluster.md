---
title: Using multiple clusters with kubectl
date: 2021-06-14
menu: topic
categories:
- snippet
tags:
- kubectl
- kubernetes
---

In order to connect to multiple [kubernetes](https://kubernetes.io/) clusters with [kubectl](https://kubernetes.io/docs/reference/kubectl/overview/), I like to define aliases like this:

```shell script
alias rancher="kubectl --kubeconfig ~/.kube/rancher.config"
alias work="kubectl --kubeconfig ~/.kube/work.config"
alias customer="kubectl --kubeconfig ~/.kube/customer.config"
```

Those aliases allow me to write things like `rancher get pods --namespace some-namespace` without worrying the wrong context is active.
