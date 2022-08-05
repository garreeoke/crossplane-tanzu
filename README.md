# crossplane-tanzu

This repo is created as an example of Upbound's Universal Crossplane (UXP) working with VMware's Tanzu Application Platform (TAP). It will utilize Crossplane within a TAP supply-chain to create and use an AWS Broker (RabbitMQ) and then use the created RabbitMQ instance in the Spring-Sensors application that is built and deployed.

## Pre-requisites
1. Create and gain access to Kubernetes cluster (tested with v1.24.2 on GKE with 12vcpu and 48GB of memory)
2. [Install Universal Crossplane](https://cloud.upbound.io/docs/uxp/install) (Upbound's Crossplane distro)
3. [Install Upbound's Official AWS Provider](https://marketplace.upbound.io/providers/upbound/provider-aws/v0.5.0)
4. Install Tanzu Application Platform
    - [General install instructions](https://docs.vmware.com/en/VMware-Tanzu-Application-Platform/1.2/tap/GUID-install-intro.html)
    - [AWS Quickstart](https://aws-quickstart.github.io/quickstart-vmware-tanzu-application-platform/#_deployment_options)
5. Access to create a Broker in AWS

## Steps to complete the demo
1. Complete pre-requisites
2. [Add a secret and provider config for your AWS account](https://cloud.upbound.io/docs/getting-started/build-control-plane/#add-your-credentials)
3. Using Crossplane, create the one API used to create everything needed on the cluster and in AWS 
    >`kubectl apply -f crossplane/`
4. Setup the supply-chain and templates needed within Tanzu Application Platform
    >`kubectl apply -f tanzu/`
5. Create the workload and get a cup of coffee
    >`tanzu apps workload create -f demo/workload-cloud.yaml`
6. Access spring-sensors and see if data is coming in
7. Cleanup it all up
    >`tanzu apps workload delete spring-sensors -y`

## What's in the Box (repository)?
- crossplane
    - composition.yaml: Composition of all the resources needed
    - definition.yaml:  Definition of the only API needed to create all the needed resources
    - claims:  directory with a test claim file
- tanzu
    - source-to-url-supply-chain-cloud.yaml: Main supply chain config
    - cluster-template-runnable.yaml: Template used within the supply chain to call the runnable
    - cluster-run-template.yaml: Template for the runnable
- demo
    - workload-cloud.yaml: The only file needed for the end-user to build and create everything on the cluster and in AWS
- experiments: files used while testing this out