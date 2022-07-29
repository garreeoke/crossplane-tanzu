# crossplane-tanzu

This repo is created as an example of Crossplane/Upbound working with a Tanzu Application Platform supply-chain to create and use an AWS Broker (RabbitMQ) and then use it in the Spring-Sensors application.

## Pre-requisites
1. Create and gain access to Kubernetes cluster (tested with v1.24.2 on GKE)
2. Install Universal Cloud Platform (Upbound's Crossplane distro)
3. Install Upbound's Official AWS Provider
4. Install Tanzu Application Platform
5. Access to create a Broker in AWS.

## Steps to complete the demo
1. Complete pre-requisites
2. Add a provider config for your AWS account
3. Using Crossplane, create the one API used to create everything needed on the cluster and in AWS 
    `kubectl apply -f crossplane/`
4. Setup the supply-chain and templates needed within tanzu
    `kubectl apply -f tanzu/`
5. Create the workload and get a cup of coffee
    ```tanzu apps workload create -f demo/workload.yaml```
6. Access spring-sensors and see if data is coming in