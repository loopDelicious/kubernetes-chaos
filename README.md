# Building resilient APIs with chaos engineering

The example that follows supports an article published on _Better Practices_. Learn more about [Building resilient APIs with chaos engineering](https://medium.com/@joycelin.codes/chaos-d3ef238ec328).

## Getting Started

This sample app is forked from Google's [Hipster Shop: Cloud-Native Microservices Demo Application](https://github.com/GoogleCloudPlatform/microservices-demo) - a web-based e-commerce app called **“Hipster Shop”** where users can browse items, add them to the cart, and purchase them. The application works on any Kubernetes cluster (such as a local one).

#### Set up Gremlin and create a Kubernetes cluster on EKS

Start with this guide to [Install Gremlin to use with Amazon EKS](https://www.gremlin.com/community/tutorials/how-to-install-and-use-gremlin-with-eks). You'll need an AWS account, the AWS CLI configured to use eksctl to create the EKS cluster, and a Gremlin account.

- Step 0 - Verify your account AWS CLI Installation
- Step 1 - Create an EKS cluster using eksctl
- Step 2 - Load up the kubeconfig for the cluster
- Step 3 - Deploy Kubernetes Dashboard
- Step 4 - Deploy a Microservice Demo application
- Step 5 - Run a Shutdown Container Attack using Gremlin (skip)

![[hipster shop](https://i.imgur.com/IWGvLNF.pngv=4&s=200)](https://i.imgur.com/IWGvLNF.png)

#### Set up Grafana and Prometheus

You'll need to a way to observe the results of your attack. You can skip this step if you have a different way to do this.

Proceed with [Monitoring Kubernetes clusters with Grafana](https://medium.com/htc-research-engineering-blog/monitoring-kubernetes-clusters-with-grafana-e2a413febefd). This particular guide starts with Google Kubernetes Engine (GKE) instead of EKS, but most of the steps are the same after you've create your cluster.

- Step 0 - Create a GKE cluster (skip)
- Step 1 - Lots and lots and lots of yaml configuration
- Step 2 - Configure your cluster settings on Grafana (see gotchas below)

![[grafana dashboard 3131](https://i.imgur.com/a0tDoJT.png)](https://i.imgur.com/a0tDoJT.png)

#### Use Postman to shut down a single container via the Gremlin API

In the Postman app, [import the template](https://learning.getpostman.com/docs/postman/launching_postman/newbutton/#templates) called `Chaos engineering` that includes the `chaosEngineering` environment, and then look for the folder called `Shut down a container`. Read the [Chaos engineering](insert link) collection documentation for step-by-step instructions.

1. Get a list of all active containers
1. Create a shutdown attack on a specific container
1. Verify app health
1. Stop the attack (if you need to)
   ![[attack in postman](https://i.imgur.com/7hrGhIg.png)](https://i.imgur.com/7hrGhIg.png)
   ![[500 error](https://i.imgur.com/ulRVM69.png)](https://i.imgur.com/ulRVM69.png)
   ![[collection runner](https://i.imgur.com/UPPy3bQ.png)](https://i.imgur.com/UPPy3bQ.png)

## Some gotchas

#### Managing Users or IAM Roles for your AWS EKS cluster

If you're using [`aws-iam-authenticator`](https://github.com/kubernetes-sigs/aws-iam-authenticator#specifying-credentials--using-aws-profiles) to manage your clusters from the CLI and have an MFA authentication requirement, you may need to [get a session token](https://docs.aws.amazon.com/cli/latest/reference/sts/get-session-token.html), and update a separate profile in your `/.aws/credentials` in order to use the CLI and build a programmatic solution.

#### Configuring your EKS cluster in Grafana

Your configuration will depend on where you've deployed your Kubernetes cluster and Grafana. I opted to have Grafana and Prometheus running on my cluster.

You can use an ingress controller to manage external access to the apps running inside the cluster like [`ingress-nginx`](https://github.com/kubernetes/ingress-nginx).

- You might also need a service that automatically creates and manages TLS certs in Kubernetes like [`cert-manager`](https://github.com/helm/charts/tree/master/stable/cert-manager) (as in [this tutorial](https://itnext.io/automated-tls-with-cert-manager-and-letsencrypt-for-kubernetes-7daaa5e0cae4)).

Additionally, you can use an existing Grafana dashboard like [this one](https://grafana.com/grafana/dashboards/3131) for an overview of all nodes in a Kubernetes Cluster. There are other options as well.

- Configuring your cluster in Grafana using the `kubernetes-app` plugin is not super well-documented, so [these notes](https://github.com/grafana/kubernetes-app/issues/35#issuecomment-407878758) might help.
- TLS certs are required for authentication using the plugin, and EKS doesn't support TLS. You can try using a different managed service like GKE (as in [this tutorial](https://medium.com/htc-research-engineering-blog/monitoring-kubernetes-clusters-with-grafana-e2a413febefd)), or many more steps will be required (if you're using the plugin).
