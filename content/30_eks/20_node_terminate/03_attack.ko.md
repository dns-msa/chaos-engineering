---
title: "3. 장애 주입 실험"
chapter: false
weight: 13
---

## 실험 시작

EKS 노드 그룹의 인스턴스들이 잘 동작하고 있는 지 확인합니다.
```sh
kubectl get nodes
```

이제, AWS FIS 서비스 페이지로 이동합니다. 내비게이션 메뉴에서 **실험 템플릿** 을 선택합니다. 실험 템플릿 목록이 나타나면 `TerminateEKSNodes`을 선택합니다. 화면 오른 쪽 상단에 있는 **작업** 단추를 누르고 이어서 나타는 메뉴 중에서 **실험 시작** 을 선택합니다. AWS FIS는 EKS 노드 그룹의 최대 70%까지 종료 시킬 수 있습니다. 한 번에 모든 노드를 종료하면 서비스에 문제가 생길 수 있기 때문입니다. 이 번 실습에서는 40%의 노드를 종료하도록 실험을 구성했습니다. 만약, 종료시킬 노드의 비율을 변경하고 싶다면 **작업** 단추를 누르고 **실험 템플릿 업데이트** 를 선택하면 됩니다. 템플릿 편집화면이 나타나면 **대상** 부분에서 원하는 값으로 조정한 다음 **저장** 을 눌러 반영합니다. 실험을 시작하고 나면, EC2 서비스 페이지에서 종료된 EKS 노드들을 볼 수 있습니다. 그리고 EKS 노드가 종료된 직후 새 인스턴스가 곧 실행되는 것을 볼 수 있습니다.

![aws-fis-terminate-eks-nodes](/images/30_eks/aws-fis-terminate-eks-nodes.png)

![aws-fis-terminate-eks-nodes-action-complete](/images/30_eks/aws-fis-terminate-eks-nodes-action-complete.png)

## 실험 결과 확인

아래 명령을 통해 일부 노드가 종료되는 것을 확인할 수 있습니다:
```sh
kubectl -n sockshop get node -w
```
```sh
NAME                                            STATUS   ROLES    AGE     VERSION
ip-10-1-1-205.ap-northeast-2.compute.internal   Ready    <none>   21m     v1.20.4-eks-6b7464
ip-10-1-9-221.ap-northeast-2.compute.internal   Ready    <none>   4m40s   v1.20.4-eks-6b7464
ip-10-1-9-221.ap-northeast-2.compute.internal   NotReady   <none>   4m40s   v1.20.4-eks-6b7464
ip-10-1-9-221.ap-northeast-2.compute.internal   NotReady   <none>   4m40s   v1.20.4-eks-6b7464
```

## 개선점 토의

Cluster Autoscaler is a tool that automatically adjusts the size of the Kubernetes cluster when one of the following conditions is true:

+ there are pods that failed to run in the cluster due to insufficient resources.
+ there are nodes in the cluster that have been underutilized for an extended period of time and their pods can be placed on other existing nodes.

Cluster Autoscaler provides integration with Auto Scaling groups. Cluster Autoscaler will attempt to determine the CPU, memory, and GPU resources provided by an EC2 Auto Scaling Group based on the instance type specified in its Launch Configuration or Launch Template. Click [here](https://github.com/kubernetes/autoscaler/tree/master/cluster-autoscaler/cloudprovider/aws) for more information.

Watch the logs to verify cluster autoscaler is installed properly. If everything looks good, we are now ready to scale our cluster.
```sh
kubectl -n kube-system logs -f deployment/cluster-autoscaler
```

Scale out pods for high availability.
```sh
cd ~/environments/fisworkshop/eks/
kubectl apply -f kubernetes/manifest/sockshop-demo-ha.yaml
```

## 그룹 토론

실험 계획과 예상 결과를 작성해보기 (5분 작성 / 10분 발표 토론)

## 재실험을 통한 가설 검증