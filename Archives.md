#### AMAZON

1. https://leetcode.com/discuss/interview-question/6272497/AMAZON-or-L5-or-BLR-or-Onsite
    #### HLD to design health monitoring system for million services
      Design:
      <img width="785" alt="image" src="https://github.com/user-attachments/assets/f11c1cb8-83e6-41be-a1eb-c58acbd5ee88" />

      Tech:
      <img width="785" alt="image" src="https://github.com/user-attachments/assets/b3bc243e-b27a-4055-b4fe-3a90de3dbe26" />

Link : https://gongybable.medium.com/system-design-design-a-monitoring-system-f0f0cbafc895
![image](https://github.com/user-attachments/assets/3d8cd3e9-c206-4018-929d-7f3475cba604)

Exporter — Pulls metrics from targets and convert them to correct format\
Push Gateway — Kron jobs to push metrics to at exit, then we can pull metrics from it\
Data retrival workers — pull data\
Time series storage — Local SSD / Remote Storage\
Query Service — visualize data\
Alert manager — to send alerts to different channels\
Service Discovery — Configuration for the targets to pull metrics from\



     

   
2. 
