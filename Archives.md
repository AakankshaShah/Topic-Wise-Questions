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

```
  How to Collect Metrics — Pull or Push
There are two models to collect data, push and pull. In monitoring system, I would always go with pull model, and the reason is as below:

Scalability Concern. Our infrastructure will keep growing, and we many have hundreds or thousands of services in the coming years. And our service usage, user base will grow too. If we go with the push model, then all these services will keep hitting our monitor service. If we have a service which processes 1M requests per second, and this service push the metrics to our monitoring service upon every request, then we will suffer from scalability issue frequently as we grow. So instead of getting called to get metrics, I would prefer to actively pull the data from the services.
Automatic Upness Monitoring — By pulling the data proactively, we can directly know if the service is alive or not. For example, if one service is not reachable, we can be aware of it immediately.
Easier Horizontal Monitoring — If we have two independent systems A and B, but one day we need to monitor some service in system B from system A. We can pull metrics from system B directly, no need to configure system B to push to system A.
Easier for Testing — We can simply spin up testing env, and copy the configuration from production, then you can pull the same metrics as prod and do testing.
Simpler High Availability — just spin up two servers with the same configuration to pull the same data to achieve HA.
Less configuration, no need to configure every service.
Base on the analysis above, my design for the pull model is below:

Our service will pull the data from the services regularly (for example every second). We need a real time monitoring system, but a lag of a couple of seconds is totally fine.
Exporters — The services should not call our monitor service to send the data. Instead, they can save the metrics to an exporter, and the data can be stored there to get pulled. So that, our monitor service will not be exhausted from getting called, and it will be more scalable. Also our monitoring system may need the data in a specific format, and the services may be designed in different technologies, and have data in different formats. So we require an exporter attached to each service, which reformats the data into the correct format for our monitor services. And our monitor will pull the data from the exporters.
Push Gateway — For cron jobs, they are not service based, but we may need to monitor the metrics from them too. So we can have a push gateway, which lives behind all the cron jobs, and the monitor can just pull the data from the gateway directly.
Exporter Design
Since we discussed the components for the Pull model, i.e., Exporter, and Push Gateway.

Some interview may question why not have multiple services hooked to one exporter. And I would always prefer one service per exporter, and the argument is below:

Operational bottleneck — the exporter will become a bottleneck if we have too many services behind it
Single point of failure, and one service pushes too much will block others
If I am only interested in the metrics of one service, I cannot get that only, I have to read all
No upness monitoring — if one service is not reachable, we will not be able to know.
Hard to get service metadata — we can store the service metadata in the exporter

```
```
   Pull:
Use Case: When services are in a controlled network or support service discovery.
Example: Prometheus scraping Kubernetes pods.
Best For:
Systems with dynamic targets.
Centralized control over metrics collection.
Low-overhead metric retrieval.
Push:
Use Case: When services are in different networks, or you need real-time updates.
Example: IoT devices pushing metrics to a central system.
Best For:
Systems with firewalls/NAT.
Real-time applications needing low-latency updates.
Scenarios where service discovery is difficult.
```



     

   
2. 
