# S3 Glacier
S3 Glacier is a storage class in Amazon S3 for long-term data archiving at a lower cost. It's ideal for backups, compliance data, and logs that are rarely accessed but must be stored securely and durably for years

## Key Features:

- Low Cost-  ~1/10th the cost of S3 Standard
- High Durability- 99.999999999% (11 9’s) durability
- Multiple Retrieval Options-	Choose between expedited (1–5 min), standard (3–5 hrs), and bulk (5–12 hrs)
- Data Encryption- AES-256 or KMS by default
- Lifecycle Integration- Automatically transition old S3 data to Glacier

## Use cases
1. Old logs older than 90 days	
Automatically archive via lifecycle policy
2. Database snapshots for compliance	
Store in Glacier or Deep Archive
3. Financial or healthcare records	
Long-term low-cost retention
4. Legal or audit documentation	
Store for 7–10 years with WORM policy

Example: Move files from S3 Standard to Glacier after 90 days (Data Transition S3 Lifecycle Policy)
You have an S3 bucket logs-prod-app/ that stores log files from your production web application.
You’re required to keep logs for 1 year, but after 90 days they are rarely accessed.

🔹 Step-by-step:
1. Store new logs in S3 Standard

2. Apply Lifecycle policy:
    Transition to Glacier after 90 days
    Delete after 365 days

```bash
{
  "Rules": [
    {
      "ID": "ArchiveOldLogs",
      "Prefix": "logs-prod-app/",
      "Status": "Enabled",
      "Transitions": [
        {
          "Days": 90,
          "StorageClass": "GLACIER"
        }
      ],
      "Expiration": {
        "Days": 365
      }
    }
  ]
}
```
