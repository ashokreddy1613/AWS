# Cloud Watch

Cloud Watch is a monitoring and observability service for AWS resources and custom applications.

Its a gate keeper for AWs account which will help in understanding, implememting, monitoring, alerting, reporting and logging.

It lets you collect logs, metrics, events, and set alarms to track performance, usage, and operational health

## Core Components 

1. Metrics
Numerical data like CPU %, memory, disk I/O, etc.
2. Logs	
Capture logs from EC2, Lambda, ECS, API Gateway, etc.
3. Alarms	
Trigger notifications/actions based on metric thresholds
4. Dashboards	
Visualize metrics in real time
5. Events 
(now called EventBridge) Respond to changes in AWS (e.g., EC2 stop/start)
6. CloudWatch Agent	
Collects memory, disk, and custom app metrics/logs

## Common Use Cases:
1. Alarm setup if **EC2 CPU > 80%**
- Go to CloudWatch-> Create Alarm
- Select Metric: EC2 > CPUUtilization
- Condition: > 80 % for 5 minutes
- Action: send SNS notification or trigger Auto-scaling

2. Monitor Disk and Memory Usage on EC2
You want to get alerts if disk or memory usage exceeds 90%.

🛠️ CloudWatch Solution:
    Install CloudWatch Agent on EC2
    Collect memory & disk metrics
    Create alarms for high usage

✅ Benefit: Prevent disk full/memory overload outages.

3. Auto-Recover Unhealthy EC2 Instances
🔹 Scenario:
You want your production EC2 server to automatically recover if it becomes unresponsive.

🛠️ CloudWatch Solution:
    Use CloudWatch Alarm on StatusCheckFailed_System metric
    Trigger EC2 recovery action

✅ Benefit: No manual restart, reduces downtime.

Many use cases like alert when CPU uage spikes, log and monitor Lambda errors
 