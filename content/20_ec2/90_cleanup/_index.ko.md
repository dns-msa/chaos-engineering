---
title: "실습 환경 정리"
chapter: false
weight: 90
---

EC2의 AutoScaling의 Warm Pool을 사용한 실험을 했다면, Warm Pool을 먼저 삭제해야 합니다. 먼저 콘솔에서 EC2 서비스로 이동한 뒤 좌측메뉴에서 Auto Scaling Groups를 선택합니다. 실습에 사용한 Auto Scaling Group을 선택하고 Instance management 탭을 선택합니다. Warm Pool이 있다면 삭제합니다.

Cloud9 환경의 터미널에서 아래의 스크립트를 실행하면 실습을 위해 배포했던 어플리케이션이 삭제됩니다.

```
cd ~/environment/fisworkshop/ec2/
./chaos-99-destroy-all.sh
```
