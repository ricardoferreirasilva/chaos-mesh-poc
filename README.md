# Description
This repository was created in order to showcase a proof-of-concept implementation of Chaos Mesh with K8s.

# Instruction
We will start by creating two namespaces: chaos-mesh to host our chaos mesh related deployables and agent plus a staging environment for running our experiments.

``kubectl apply -f ./namespaces``

Next we will install chaos-mesh and verify its installation. You need helm locally for this step.

`helm repo add chaos-mesh https://charts.chaos-mesh.org`

`helm install chaos-mesh chaos-mesh/chaos-mesh -n=chaos-mesh --version 2.7.0`

`kubectl get po -n chaos-mesh`

Next we need to create cluster wide roles for the chaos-mesh agent.

`kubectl apply -f roles`

Switch to the default namespace `kubens default`
generate a token 
`kubectl create token account-cluster-manager` and save it

The chaos-mesh dashboard can be accessed by doing a simple port forward.
`kubectl port-forward chaos-dashboard-7c66c9f685-2wkh4 2333:2333` you can past your token on the dashboard.

We will now deploy an hello-world app on the staging environment in order to run experiments on it.

`kubectl apply -f ./hello-world`

This app will be available on localhost:30001 due to the NodePort.
We can now use the dashboard or manifest files to create experiments for it. For example:

`kubectl apply -f ./experiments/hello-world-pod-failure.yaml` will simulate pod failures for all pods in the staging environment for the next 2 minutes.
