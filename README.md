lego-acme compiled
==================
This is a lazy way for me to get certificates using Route53. Precompiled lego runs on Windows or Linux boxes.

Make sure your DNS situation is good. Some ISPs cache DNS records for a long time, which will make this unusable.

```batch
set LEGO_EXPERIMENTAL_CNAME_SUPPORT=true
set AWS_REGION=us-east-1
set AWS_ACCESS_KEY_ID=<my_id>
set AWS_SECRET_ACCESS_KEY=<my_key>
lego -a --email "websecurity@example.com" --domains example.com --domains "*.example.com" --dns="route53" run
dir .lego\certificates
```

For Linux:

```bash
LEGO_EXPERIMENTAL_CNAME_SUPPORT=true \
AWS_REGION=us-east-1 \
AWS_ACCESS_KEY_ID=<my_id> \
AWS_SECRET_ACCESS_KEY=<my_key> \
lego -a --email "websecurity@example.com" --domains example.com --domains "*.example.com" --dns="route53" run
ls .lego
```

Route 53 configuration
----------------------
Min. policy for the dns bot:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "route53:ListHostedZonesByName"
      ],
      "Resource": [
        "*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "route53:GetChange"
      ],
      "Resource": [
        "arn:aws:route53:::hostedzone/*",
        "arn:aws:route53:::change/*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "route53:ListResourceRecordSets",
        "route53:ChangeResourceRecordSets"
      ],
      "Resource": [
        "arn:aws:route53:::hostedzone/<**HOSTZONE_ID**>"
      ]
    }
  ]
}
```
