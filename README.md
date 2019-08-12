# Title

[link to better practices]

### Attribution

This sample app is forked from [Hipster Shop: Cloud-Native Microservices Demo Application](https://github.com/GoogleCloudPlatform/microservices-demo). The example is a web-based e-commerce app called **“Hipster Shop”** where users can browse items, add them to the cart, and purchase them. **Google uses the application to demonstrate use of technologies**. The application works on any Kubernetes cluster (such as a local one).

This example begins with this tutorial to [install and use Gremlin with AWS EKS](https://www.gremlin.com/community/tutorials/how-to-install-and-use-gremlin-with-eks).

### Picking up where the Gremlin tutorial leaves off

We'll be using Postman with the Gremlin API to shut down a single container. Spoiler alert . . . this is so we can easily automate these chaos tests with our continuous integration pipeline.

Import the Postman template called Chaos Engineering and look for the folder called Infrastructure attack [insert link] if you're following along.
