# 📊 EC2 Log Management & Monitoring Guide

## Overview
This guide explains how to:
- Send logs from EC2 to S3
- Monitor CPU metrics
- Set up CloudWatch alarms
- Automate the entire process

## 🚨 Prerequisites
- EC2 instance running
- S3 bucket for log storage
- IAM roles and permissions
- CloudWatch access
- AWS CLI configured

## 🔧 Implementation Steps

### 1. IAM Role Configuration
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:GetObject",
                "cloudwatch:PutMetricData",
                "cloudwatch:PutMetricAlarm",
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:PutLogEvents"
            ],
            "Resource": [
                "arn:aws:s3:::your-log-bucket/*",
                "arn:aws:logs:*:*:*",
                "arn:aws:cloudwatch:*:*:*"
            ]
        }
    ]
}
```

### 2. Log Collection Script
```bash
#!/bin/bash

# Configuration
LOG_DIR="/var/log"
S3_BUCKET="your-log-bucket"
INSTANCE_ID=$(curl -s http://169.254.169.254/latest/meta-data/instance-id)
DATE=$(date +%Y-%m-%d)
LOG_FILE="ec2-logs-${INSTANCE_ID}-${DATE}.tar.gz"

# Collect logs
tar -czf /tmp/${LOG_FILE} ${LOG_DIR}/*

# Upload to S3
aws s3 cp /tmp/${LOG_FILE} s3://${S3_BUCKET}/logs/${LOG_FILE}

# Cleanup
rm /tmp/${LOG_FILE}
```

### 3. CPU Monitoring Script
```bash
#!/bin/bash

# Configuration
INSTANCE_ID=$(curl -s http://169.254.169.254/latest/meta-data/instance-id)
CPU_THRESHOLD=80

# Get CPU usage
CPU_USAGE=$(top -bn1 | grep "Cpu(s)" | awk '{print $2}' | cut -d. -f1)

# Send metric to CloudWatch
aws cloudwatch put-metric-data \
    --metric-name CPUUtilization \
    --namespace EC2Metrics \
    --value ${CPU_USAGE} \
    --unit Percent \
    --dimensions InstanceId=${INSTANCE_ID}
```

### 4. CloudWatch Alarm Configuration
```bash
# Create CloudWatch Alarm
aws cloudwatch put-metric-alarm \
    --alarm-name "HighCPUUtilization" \
    --alarm-description "Alarm when CPU exceeds 80%" \
    --metric-name CPUUtilization \
    --namespace EC2Metrics \
    --statistic Average \
    --period 300 \
    --threshold 80 \
    --comparison-operator GreaterThanThreshold \
    --dimensions Name=InstanceId,Value=${INSTANCE_ID} \
    --evaluation-periods 2 \
    --alarm-actions arn:aws:sns:region:account-id:notification-topic
```

### 5. Automation Setup (crontab)
```bash
# Add to crontab
*/5 * * * * /path/to/cpu-monitor.sh
0 0 * * * /path/to/log-collector.sh
```

## 🛡️ Security Best Practices

### 1. IAM Security
- 🔒 Use least privilege principle
- 🔑 Regular role reviews
- 📜 Document permissions
- 🔐 Encrypt sensitive data

### 2. Log Security
- 🔐 Enable S3 encryption
- 🔒 Use VPC endpoints
- 📝 Implement log rotation
- 🗑️ Set up log retention

### 3. Monitoring Security
- 🔒 Secure CloudWatch access
- 🔐 Encrypt metrics
- 📊 Monitor access patterns
- 🚨 Alert on suspicious activity

## ⚠️ Error Handling

### 1. Common Issues
- Log collection failures
- S3 upload errors
- Metric collection issues
- Alarm notification failures

### 2. Troubleshooting Steps
1. ✅ Check IAM permissions
2. 🔍 Verify script execution
3. 📊 Review CloudWatch logs
4. 🔑 Validate credentials

### 3. Recovery Procedures
- 🔄 Implement retry logic
- 📝 Log all operations
- ⚡ Handle script failures
- 🚨 Alert on critical errors

## 📊 Monitoring Setup

### 1. CloudWatch Dashboard
```bash
# Create dashboard
aws cloudwatch put-dashboard \
    --dashboard-name "EC2Monitoring" \
    --dashboard-body '{
        "widgets": [
            {
                "type": "metric",
                "properties": {
                    "metrics": [
                        ["EC2Metrics", "CPUUtilization"]
                    ],
                    "period": 300,
                    "stat": "Average",
                    "region": "region",
                    "title": "CPU Utilization"
                }
            }
        ]
    }'
```

### 2. Log Analysis
```bash
# Query CloudWatch Logs
aws logs filter-log-events \
    --log-group-name /var/log/syslog \
    --filter-pattern "ERROR"
```

## 📚 Additional Resources

- [AWS CloudWatch Documentation](https://docs.aws.amazon.com/cloudwatch/)
- [AWS S3 Documentation](https://docs.aws.amazon.com/s3/)
- [AWS EC2 Documentation](https://docs.aws.amazon.com/ec2/)
- [AWS IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)

## 🤝 Contributing

Feel free to contribute to this guide by:
1. Forking the repository
2. Creating a feature branch
3. Submitting a pull request

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details. 