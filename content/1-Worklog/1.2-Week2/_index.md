---
title: "Week 2 Worklog"
date: 2026-04-27
weight: 2
chapter: false
pre: " <b>1.2.</b> "
---

### Week 2 Objectives:

- Get familiar with EC2 — the foundational compute service of AWS.
- Complete the architecture diagram and sequence diagram for the entire HireFlow AI system.

### Tasks to be carried out this week:


| Day | Task                                                                                                                                                                                                                               | Start Date | Completion Date | Reference Material                                                                           |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | -------------------------------------------------------------------------------------------- |
| 2   | Complete the first EC2 lab: launch a `t2.micro` Ubuntu 22.04 instance, create a `.pem` key pair, configure a Security Group to open port 22 only, successfully SSH into the instance, install Nginx, and set up a $1 Budget alert. | 27/04/2026 | 27/04/2026      | [Giới thiệu về Amazon EC2 :: GIỚI THIỆU VỀ AMAZON EC2](https://000004.awsstudygroup.com/vi/) |
| 3   | **Deploy Application with Auto Scaling:**• Prepare EC2, create AMI & Launch Template.• Create Target Group, configure Load Balancer.• Connect the application to RDS.                                                           | 28/04/2026 | 28/04/2026      | [https://000006.awsstudygroup.com/](https://000006.awsstudygroup.com/)                       |
| 4   | **Advanced Auto Scaling:**• Test the application via Load Balancer.• Configure Dynamic Scaling / Scheduled Scaling.• Perform load testing and observe Auto Scaling in action.                                                   | 29/04/2026 | 29/04/2026      | [https://000006.awsstudygroup.com/](https://000006.awsstudygroup.com/)                       |




### Week 2 Achievements:

- Understood and successfully deployed Amazon RDS: created a DB instance, configured Security Groups, and connected the application to the database.
- Learned how to package applications via AMI/Launch Template and deploy a Load Balancer to distribute incoming traffic.
- Successfully configured an Auto Scaling Group, understood scaling strategies (dynamic/scheduled), and tested the system's auto-scaling capabilities.
- Grasped the resource cleanup process to prevent unexpected AWS charges.

