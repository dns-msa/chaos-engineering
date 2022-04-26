---
title: "Cleanup"
chapter: false
weight: 90
---

If you used EC2 Auto Scaling Warm Pool, you must first delete the Warm Pool. First, go to the EC2 service in the console and select Auto Scaling Groups from the left navigation bar. Select the Auto Scaling Group used in the lab and select the Instance management tab. If there is a warm pool, delete it.

Run the cleanup script on your Cloud9 workspace to remove all resources we've created in this module:
```
cd ~/environment/fisworkshop/ec2/
./chaos-99-destroy-all.sh
```
