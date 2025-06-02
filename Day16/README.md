# CloudFront

CloudFront is a global CDN that caches and delivers content from edge locations to reduce latency and improve performance for web applications. It integrates with services like S3, ALB, API Gateway, and supports HTTPS, WAF, and signed URLs."

⚙️ Example Setup: S3 Static Website with CloudFront
- Upload your site to S3 bucket

- Make it public (or use signed URLs)

- Create a CloudFront distribution:
    Origin: S3 bucket
    Cache behavior: GET/HEAD allowed
    Enable HTTPS

- (Optional) Add a custom domain + ACM certificate

- Now users get fast, secure delivery of your website globally.

Amazon CloudFront is a CDN that accelerates content delivery by caching it at edge locations worldwide. used it to serve static websites from S3 with HTTPS, integrate it with ALB and API Gateway for dynamic sites, and protect content using signed URLs and WAF."

