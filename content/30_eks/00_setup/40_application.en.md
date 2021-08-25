---
title: "Deploy Application"
chapter: false
weight: 40
---

## Update kubeconfig

In order to interact with your cluster through kubectl, you can use the aws eks update-kubeconfig AWS CLI command to configure your local kubeconfig. The EKS module will define a CloudFormation output in your stack which contains the command to run. For example:
```
Outputs:
ClusterConfigCommand43AAE40F = aws eks update-kubeconfig --name cluster-xxxxx --role-arn arn:aws:iam::112233445566:role/yyyyy
```

Copy the output value and execute that aws eks update-kubeconfig command in your terminal to create or update a local kubeconfig context:
```sh
aws eks update-kubeconfig --name cluster-xxxxx --role-arn arn:aws:iam::112233445566:role/yyyyy
```

## Deploy Application

For this lab, we picked up the Sock Shop application. Sock Shop is a microservices architecture sample application that Weaveworks initially developed. They made it open source so it can be used by other organizations for learning and demonstration purposes.

Create the namespace and deploy application.
```sh
cd ~/environments/fisworkshop/eks/
kubectl apply -f kubernetes/manifest/sockshop-demo.yaml
```

Verify that the pod came up fine (ensure nothing else is running on port 8079):
```sh
kubectl -n sockshop get pod -l name=front-end
```

The output will be something like this:
```sh
NAME                         READY   STATUS    RESTARTS   AGE
front-end-7b8bcd59cb-wd527   1/1     Running   0          9s
```

### Local Workspace
In your local workspace, connect through a proxy to access your application's endpoint.

```sh
kubectl -n sockshop port-forward svc/front-end 8080:80
```
Open http://localhost:8080 on your web browser. This shows the Sock Shop main page.

### Cloud9
In your Cloud9 IDE, run the application.

```sh
kubectl -n sockshop port-forward svc/front-end 8080:80
```
Click Preview and Preview Running Application. This opens up a preview tab and shows the Sock Shop main page.

![sockshop-preview](/images/30_eks/weaveworks-sockshop-frontend.png)

## Congratulations!

:tada: You’ve deployed the sample application on your cluster.