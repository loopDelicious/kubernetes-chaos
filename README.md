# Building resilient APIs with chaos engineering

The example that follows supports an article published on _[Better Practices](https://medium.com/better-practices)_. Learn more about chaos engineering [here]().

[link to better practices]

This sample app is forked from Google's [Hipster Shop: Cloud-Native Microservices Demo Application](https://github.com/GoogleCloudPlatform/microservices-demo) - a web-based e-commerce app called **“Hipster Shop”** where users can browse items, add them to the cart, and purchase them. The application works on any Kubernetes cluster (such as a local one).

## Getting Started

#### Set up Gremlin and create a Kubernetes cluster on EKS

Start with this guide to [Install Gremlin to use with Amazon EKS](https://www.gremlin.com/community/tutorials/how-to-install-and-use-gremlin-with-eks). You'll need an AWS account, the AWS CLI configured to use eksctl to create the EKS cluster, and a Gremlin account.

- Step 0 - Verify your account AWS CLI Installation
- Step 1 - Create an EKS cluster using eksctl
- Step 2 - Load up the kubeconfig for the cluster
- Step 3 - Deploy Kubernetes Dashboard
- Step 4 - Deploy a Microservice Demo application
- Step 5 - Run a Shutdown Container Attack using Gremlin (skip)

[hipster shop][gremlin dashboard][kubernetes dashboard]

#### Set up Grafana and Prometheus

Proceed with [Monitoring Kubernetes clusters with Grafana](https://medium.com/htc-research-engineering-blog/monitoring-kubernetes-clusters-with-grafana-e2a413febefd). This particular guide starts with Google Kubernetes Engine (GKE) instead of EKS, but the steps are the same after you've create your cluster.

- Step 0 - Create a GKE cluster (skip)
- Step 1 - Lots and lots and lots of yaml configuration
- Step 2 - Configure your cluster settings on Grafana

[grafana config][grafana dashboard]

#### Use Postman to shut down a single container via the Gremlin API

In the Postman app, [import the template](https://learning.getpostman.com/docs/postman/launching_postman/newbutton/#templates) called Chaos engineering and look for the folder called Infrastructure attack. Read the [Chaos engineering](insert link) collection documentation for step-by-step instructions.

1. Get a list of our active containers
1. Start an attack by shutting down a specified container
1. Stop the attack (only if the performance of the node is not as expected)

[screenshots of postman and grafana]

## A few gotchas

#### Managing Users or IAM Roles for your AWS EKS cluster

If you're using [`aws-iam-authenticator`](https://github.com/kubernetes-sigs/aws-iam-authenticator#specifying-credentials--using-aws-profiles) to manage your clusters from the CLI and have an MFA authentication requirement, you may need to [get a session token](https://docs.aws.amazon.com/cli/latest/reference/sts/get-session-token.html), and update a separate profile in your `/.aws/credentials`.

#### Configuring your EKS cluster in Grafana

Configuring your Kubernetes cluster in Grafana is not super well-documented, so [these notes](https://github.com/grafana/kubernetes-app/issues/35#issuecomment-407878758) might help. If you're using the kubernetes-app plugin, it requires TLS certs, and EKS doesn't support TLS. You can try using a different managed service like GKE (as in [this tutorial](https://medium.com/htc-research-engineering-blog/monitoring-kubernetes-clusters-with-grafana-e2a413febefd))), create your own Grafana dashboard, or use an ingress controller to manage external access to the apps running inside the cluster like [`ingress-nginx`](https://github.com/kubernetes/ingress-nginx) in conjunction with a service that automatically creates and manages TLS certs in Kubernetes like [`cert-manager`](https://github.com/helm/charts/tree/master/stable/cert-manager) (as in [this tutorial](https://itnext.io/automated-tls-with-cert-manager-and-letsencrypt-for-kubernetes-7daaa5e0cae4)).
