# Getting Started with Jenkins-X for EKS

## Pre-requisite

- Macbook
- brew tap jenkins-x/jx

```
brew install jx
```

## Syntax

```
Examples:
  # create a cluster on Google Cloud
  jx create cluster gke --skip-installation

  # create a cluster on AWS via EKS
  jx create cluster eks --skip-installation
Available Commands:
  create cluster aks Create a new Kubernetes cluster on AKS: Runs on Azure
  create cluster eks Create a new Kubernetes cluster on AWS using EKS
  create cluster gke Create a new Kubernetes cluster on GKE: Runs on Google Cloud
  create cluster iks Create a new kubernetes cluster on IBM Cloud Kubernetes Services

Usage:
  jx create cluster [kubernetes provider] [flags] [options]
```

## Installing Jenkins X on EKS

```
jx create cluster eks --skip-installation
```

```
Missing required dependencies, deselect to avoid auto installing
- eksctl
- aws-iam-authenticator
- kubectl
- helm
```

## Installing eksctl

```
Downloading https://github.com/weaveworks/eksctl/releases/download/0.11.1/eksctl_Darwin_amd64.tar.gz to /Users/ajeetraina/.jx/bin/eksctl.tar.gz...
Downloaded /Users/ajeetraina/.jx/bin/eksctl.tar.gz
Installing aws-iam-authenticator
Downloading https://amazon-eks.s3-us-west-2.amazonaws.com/1.12.7/2019-03-27/bin/darwin/amd64/aws-iam-authenticator to /Users/ajeetraina/.jx/bin/aws-iam-authenticator...
Downloaded /Users/ajeetraina/.jx/bin/aws-iam-authenticator
Installing kubectl
Downloading https://storage.googleapis.com/kubernetes-release/release/v1.13.2/bin/darwin/amd64/kubectl to /Users/ajeetraina/.jx/bin/kubectl...
Downloaded /Users/ajeetraina/.jx/bin/kubectl
Installing helm
Downloading https://get.helm.sh/helm-v2.12.2-darwin-amd64.tar.gz to /Users/ajeetraina/.jx/bin/helm.tgz...
Downloaded /Users/ajeetraina/.jx/bin/helm.tgz
Installing helm secrets plugin
WARNING: Failed to retrieve team settings: failed to setup the dev environment for namespace 'default': the server could not find the requested resource (post environments.jenkins.io) - falling back to default settings...
Creating EKS cluster - this can take a while so please be patient...
You can watch progress in the CloudFormation console: https://console.aws.amazon.com/cloudformation/
[ℹ]  eksctl version 0.11.1
[ℹ]  using region us-east-2
[ℹ]  setting availability zones to [us-east-2a us-east-2b us-east-2c]
[ℹ]  subnets for us-east-2a - public:192.168.0.0/19 private:192.168.96.0/19
[ℹ]  subnets for us-east-2b - public:192.168.32.0/19 private:192.168.128.0/19
[ℹ]  subnets for us-east-2c - public:192.168.64.0/19 private:192.168.160.0/19
[ℹ]  nodegroup "ng-623234c1" will use "ami-082bb518441d3954c" [AmazonLinux2/1.14]
[ℹ]  using Kubernetes version 1.14
[ℹ]  creating EKS cluster "beautiful-creature-1591004252" in "us-east-2" region with un-managed nodes
[ℹ]  will create 2 separate CloudFormation stacks for cluster itself and the initial nodegroup
[ℹ]  if you encounter any issues, check CloudFormation console or try 'eksctl utils describe-stacks --region=us-east-2 --cluster=beautiful-creature-1591004252'
[ℹ]  CloudWatch logging will not be enabled for cluster "beautiful-creature-1591004252" in "us-east-2"
[ℹ]  you can enable it with 'eksctl utils update-cluster-logging --region=us-east-2 --cluster=beautiful-creature-1591004252'
[ℹ]  Kubernetes API endpoint access will use default of {publicAccess=true, privateAccess=false} for cluster "beautiful-creature-1591004252" in "us-east-2"
[ℹ]  2 sequential tasks: { create cluster control plane "beautiful-creature-1591004252", create nodegroup "ng-623234c1" }
[ℹ]  building cluster stack "eksctl-beautiful-creature-1591004252-cluster"
[ℹ]  deploying stack "eksctl-beautiful-creature-1591004252-cluster"

[✔]  EKS cluster "beautiful-creature-1591004252" in "us-east-2" region is ready

Initialising cluster ...
eks cluster created. Skipping Jenkins X installation.
```

```
[Captains-Bay]🚩 >  kubectl cluster-info
Kubernetes master is running at https://BE4A3B69D0229AE5AD01A0915EFFBD08.yl4.us-east-2.eks.amazonaws.com
CoreDNS is running at https://BE4A3B69D0229AE5AD01A0915EFFBD08.yl4.us-east-2.eks.amazonaws.com/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
[Captains-Bay]🚩 >
```


## Installing Jenkins X

```
[Captains-Bay]🚩 >  cat <<EOF > myvalues.yaml
> monocular:
>   api:
>     resources:
>       limits:
>         memory: 2Gi
>   mongodb:
>     mongodbDatabase: ""
> jenkins:
>   Master:
>     Cpu: "2000m"
>     Memory: "4096Mi"
> EOF
[Captains-Bay]🚩 >
```

Installing Jenkins X

```
[Captains-Bay]🚩 >  jx install --provider=eks --default-environment-prefix=elotl-cloud
Command "install" is deprecated, it will be removed on Jun 1 2020. We now highly recommend you use jx boot instead. Please check https://jenkins-x.io/docs/getting-started/setup/boot/ for more details.
? Configured Jenkins installation type: Serverless Jenkins X Pipelines with Tekton
Namespace jx created
Context "iam-root-account@beautiful-creature-1591004252.us-east-2.eksctl.io" modified.
Git configured for user: jenkins-x-bot and email jenkins-x@googlegroups.com
Helm installed and configured
? No existing ingress controller found in the kube-system namespace, installing one: Yes
Using local value overrides file /Users/ajeetraina/myvalues.yaml
Using helm values file: /var/folders/mq/z4bncmrs28sbtktxpck2whcm0000gn/T/ing-values-766749713

Waiting for external loadbalancer to be created and update the nginx-ingress-controller service in kube-system namespace
External loadbalancer created
Waiting to find the external host name of the ingress controller Service in namespace kube-system with name jxing-nginx-ingress-controller

On AWS we recommend using a custom DNS name to access services in your Kubernetes cluster to ensure you can use all of your Availability Zones
If you do not have a custom DNS name you can use yet, then you can register a new one here: https://console.aws.amazon.com/route53/home?#DomainRegistration:
? Would you like to register a wildcard DNS ALIAS to point at this ELB address?  Yes

? Enter your custom domain that we can use to setup a Route 53 ALIAS record to point at the ELB host: a8ef316d4a40811ea9f560a448cc2ced-0bb0b6f20b00c479.elb.us-east-2.amazonaws.com
? Your custom DNS name:
```

# Addng Custom DNS Name

During the install process, jx will ask questions for customizing your setup. One of them is about the wildcard DNS alias that will point at the main Jenkins X load balancer. You can use the free service nip.io for setting this up; check the hostname of the ELB:

```
[Captains-Bay]🚩 >  kubectl get svc -n kube-system jxing-nginx-ingress-controller
NAME                             TYPE           CLUSTER-IP      EXTERNAL-IP                                                                     PORT(S)                      AGE
jxing-nginx-ingress-controller   LoadBalancer   10.100.161.36   a8ef316d4a40811ea9f560a448cc2ced-0bb0b6f20b00c479.elb.us-east-2.amazonaws.com   80:30415/TCP,443:31075/TCP   3m27s
[Captains-Bay]🚩 >  host a8ef316d4a40811ea9f560a448cc2ced-0bb0b6f20b00c479.elb.us-east-2.amazonaws.com
a8ef316d4a40811ea9f560a448cc2ced-0bb0b6f20b00c479.elb.us-east-2.amazonaws.com has address 3.21.153.172
a8ef316d4a40811ea9f560a448cc2ced-0bb0b6f20b00c479.elb.us-east-2.amazonaws.com has address 3.23.226.40
[Captains-Bay]🚩 >  host 3.21.153.172.nip.io
3.21.153.172.nip.io has address 3.21.153.172
[Captains-Bay]🚩 >
```

So you need to add 3.21.153.172.nip.io as custom DNS

```
? Your custom DNS name:  3.21.153.172.nip.io
About to insert/update DNS CNAME record into HostedZone /hostedzone/Z03774623V5NETKP079KH with wildcard *.3.21.153.172.nip.io pointing to a8ef316d4a40811ea9f560a448cc2ced-0bb0b6f20b00c479.elb.us-east-2.amazonaws.com
Updated HostZone ID /hostedzone/Z03774623V5NETKP079KH successfully
nginx ingress controller installed and configured
Set up a Git username and API token to be able to perform CI/CD
Creating a local Git user for github.com server
? github.com username:



? API Token: ****************************************
Select the CI/CD pipelines Git server and user
? Do you wish to use github.com as the pipelines Git server: (Y/n)
Then COPY the token and enter it below:

? API Token: ****************************************
Setting the pipelines Git server https://github.com and user name collabnix.
Enumerating objects: 1440, done.
Total 1440 (delta 0), reused 0 (delta 0), pack-reused 1440
Using local value overrides file /Users/ajeetraina/myvalues.yaml
? Defaulting workload build pack: Kubernetes Workloads: Automated CI+CD with GitOps Promotion
Setting the team build pack to kubernetes-workloads repo: https://github.com/jenkins-x-buildpacks/jenkins-x-kubernetes.git ref: master
Installing jx into namespace jx
Installing jenkins-x-platform version: 2.0.2259
WARNING: waiting for install to be ready, if this is the first time then it will take a while to download images
WARNING: deployment jenkins failed to become ready in namespace jx
Jenkins X deployments ready in namespace jx
Configuring the TeamSettings for ImportMode Jenkinsfile
Configure Jenkins API Token
waiting up to 0s for the Jenkins server to be healthy at URL http://jenkins.jx.3.21.153.172.nip.io/login


```