# README 

## Prerequisites
* Kubernetes
* Helm version 3 

## Summary 
* Download Kube-prometheus-stack helm chart
* Download rabbitmq helm chart
* Edit some files.

## How to use

### 1. Download kube-prometheus-stack by Artifacthub

## [Artifacthub](https://artifacthub.io/packages/helm/prometheus-community/kube-prometheus-stack)

* helm repo add prometheus-community https://prometheus-community.github.io/helm-charts

* helm repo update

* helm pull prometheus-community/kube-prometheus-stack

* Edit kube-prometheus-stack/values.yaml : monitoring-platform [label you want to set]

![image](https://user-images.githubusercontent.com/50174803/136752452-b95bad50-5003-40b1-a672-e642985635a2.png)

* helm install [release_name] . 

### 2. Download rabbitmq by exporterhub

## [exporterhub](https://exporterhub.io/)  

* Edit helm-chart/rabbitmq/values.yaml : monitoring-platform [Same as the value set above]
![image](https://user-images.githubusercontent.com/50174803/136753510-c1aa767b-32ad-4376-a657-d84db708bfec.png)

* helm install [release_name] . 