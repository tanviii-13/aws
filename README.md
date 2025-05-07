# aws
git clone https://github.com/your-username/git-demo.git
cd git-demo
echo "Hello Git!" > hello.txt
git add hello.txt
git commit -m "Initial commit with hello.txt"
git push origin main
git fetch origin
git pull origin main
git checkout -b new-feature
echo "More text" >> hello.txt
git add hello.txt
git reset --hard HEAD
git checkout main
git branch -d new-feature
git log --oneline --graph --all

exp3
git clone https://github.com/your-username/sample-project.git
cd sample-project
echo "This line was added for testing." >> README.md
git add README.md
git commit -m "Updated README with test line"
git push origin main
git log --oneline --graph --decorate

## ✅ *Step-by-Step Brute-Force Detection Simulation*

---

### *1. Launch an EC2 Instance*

* Launch a small EC2 instance (e.g., Amazon Linux 2).
* Ensure *SSH (port 22)* is allowed from *your IP only* in the security group.

---

### *2. Simulate Brute-Force Attempts*

Use a loop to simulate login failures from your terminal.

bash
for i in {1..5}
do
  ssh -i fakekey.pem ec2-user@<public-ip>
done


📌 Replace <public-ip> with your EC2 instance’s IP.
This will produce multiple failed authentication attempts (key not accepted).

---

### *3. Enable CloudTrail (if not already enabled)*

* Go to *AWS CloudTrail*
* Create a trail:

  * Apply trail to *all regions*
  * Store logs in a *new or existing S3 bucket*
  * Enable *management and data events*
  * Turn on *log file validation*

CloudTrail logs *API access, not SSH activity directly — but can still record **ConsoleLogin* and EC2 actions.

---

### *4. Enable CloudWatch Logs for EC2*

To capture SSH logs:

* *Install and configure CloudWatch Agent* on EC2 instance:

#### a. Install CloudWatch Agent:

bash
sudo yum install amazon-cloudwatch-agent -y


#### b. Create a config to monitor /var/log/secure:

json
{
  "logs": {
    "logs_collected": {
      "files": {
        "collect_list": [
          {
            "file_path": "/var/log/secure",
            "log_group_name": "brute-force-log-group",
            "log_stream_name": "secure-log-stream"
          }
        ]
      }
    }
  }
}


Save as cw-config.json and then run:

bash
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl \
  -a fetch-config \
  -m ec2 \
  -c file:cw-config.json \
  -s


---

### *5. Trigger CloudWatch Alert*

Now configure a *CloudWatch Metric Filter*:

1. Go to *CloudWatch → Log Groups → brute-force-log-group*
2. Choose *Create Metric Filter*
3. Filter pattern:

   
   Failed password for
   
4. Create a metric name like BruteForceSSHFailed
5. Then set up an *Alarm*:

   * Threshold: > 3 failed attempts in 5 minutes
   * Action: Send SNS notification (email)
Here are two effective and demonstrable methods to *block access to ChatGPT (or similar AI services)* on a system using commonly available tools: the hosts file and *Windows Defender Firewall*. Both methods are local, easy to implement, and verifiable.

Here’s a complete guide to perform Task 10: Create a new IAM user john_doe with limited permissions, and verify the access levels as required.

✅ Step-by-Step Instructions
🔧 1. Create IAM User john_doe
Go to AWS Management Console → IAM → Users → Add users

User name: john_doe

Access type:

✅ Programmatic access (for CLI/SDK)

✅ AWS Management Console access

Set a custom password (or auto-generate one)

Optionally require password reset on first login

📸 Screenshot: IAM user creation screen

🔐 2. Attach Required Policies
Choose Attach policies directly, and select:

✅ AmazonS3ReadOnlyAccess

✅ AmazonEC2FullAccess

📸 Screenshot: Policy selection screen

🧾 3. Finish and Save Credentials
Save the Access Key ID and Secret Access Key if using programmatic access

Save Console sign-in URL, username, and password for john_doe

📸 Screenshot: Confirmation with console URL and credentials

👤 4. Sign In as john_doe
Visit the IAM user sign-in link (e.g., https://your-account-id.signin.aws.amazon.com/console)

Log in using john_doe credentials

📸 Screenshot: IAM console logged in as john_doe

🔍 5. Verify EC2 Access
Go to EC2 Console as john_doe:

✅ Try launching a new EC2 instance

✅ Start, stop, or terminate existing instances

📸 Screenshot: EC2 Dashboard working for john_doe

🔒 6. Verify S3 Restrictions
Go to S3 Console:

✅ Able to view/list all S3 buckets

❌ Try to upload or delete a file → should be denied
### ✅ Verify:

* Try accessing chat.openai.com or using an AI-based app
* It should fail or timeout

* 1. Define the Policy
Assume you want to allow access to bucket-a and bucket-b only.

Here’s a custom IAM policy:

json
Copy
Edit
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowSpecificS3Buckets",
      "Effect": "Allow",
      "Action": [
        "s3:ListBucket",
        "s3:GetObject",
        "s3:PutObject",
        "s3:DeleteObject"
      ],
      "Resource": [
        "arn:aws:s3:::bucket-a",
        "arn:aws:s3:::bucket-a/*",
        "arn:aws:s3:::bucket-b",
        "arn:aws:s3:::bucket-b/*"
      ]
    },
    {
      "Sid": "DenyAllOtherServices",
      "Effect": "Deny",
      "NotAction": [
        "s3:ListBucket",
        "s3:GetObject",
        "s3:PutObject",
        "s3:DeleteObject"
      ],
      "Resource": "*"
    }
  ]
}
🧾 2. Create the Policy in IAM
Go to IAM → Policies → Create policy

Choose the JSON tab

Paste the policy above

Click Next → Name the policy
Example: AccessSpecificS3Only

Click Create policy

📸 Screenshot to take: Policy creation summary page

👥 3. Attach Policy to User/Group
Go to Users → [select user] → Permissions → Add permissions

Choose Attach policies directly

Select your custom policy AccessSpecificS3Only

🧪 4. Verify Access
✅ The user should be able to access only bucket-a and bucket-b

❌ Trying to access EC2, Lambda, or other services will result in Access Denied

📸 Suggested screenshots:

User trying to list or upload in bucket-a (✅ success)

User trying to access EC2 or another bucket (❌ denied)

📝 Report Summary Template

Task 9 – IAM Policy for Specific S3 Bucket Access
Custom Policy Created: AccessSpecificS3Only

Permissions Summary:

✅ Allowed: bucket-a, bucket-b (read/write/delete)

❌ Denied: All other S3 buckets and AWS service
✅ Task 8 – Modify EC2 Security Group for Specific HTTP and HTTPS Access
Step-by-Step Instructions
🔧 1. Locate the Security Group
Go to AWS Console → EC2 → Instances

Select your instance → Scroll to Security → Click the Security Group ID

This opens the Security Groups configuration

📸 Screenshot: Security Group details page

🌐 2. Modify Inbound Rules
Click Edit inbound rules:

Rule 1: Restrict HTTP (Port 80) to Specific IPs
Type: HTTP

Protocol: TCP

Port Range: 80

Source: (Your IPs or internal network IPs, e.g., 192.168.1.0/24 or 10.0.0.0/16)

Description: Internal Network Access Only

Rule 2: Allow HTTPS (Port 443) from All IPs
Type: HTTPS

Protocol: TCP

Port Range: 443

Source: 0.0.0.0/0

Description: Allow all IPs for HTTPS

📸 Screenshot: Inbound Rules table after modification

✅ 3. Save and Verify Access
Click Save rules.

Test access:

HTTP (80) → Only accessible from specified IP addresses

HTTPS (443) → Accessible from any IP address

🧪 4. Verification
From a system inside the allowed IP range, open:

bash
Copy
Edit
curl http://<EC2-Public-IP>
From a system outside the allowed IP range, it should time out or give 403 Forbidden.

To test HTTPS:

bash
Copy
Edit
curl https://<EC2-Public-IP>
This should respond normally from any IP address.

📸 Screenshots:

Curl results from inside and outside allowed IPs

AWS Security Group configuration

📝 Report Summary Template
Task 8 – EC2 Security Group Configuration
Modifications Applied:

HTTP (Port 80) restricted to specific IP addresses (192.168.1.0/24)

HTTPS (Port 443) open to the world (0.0.0.0/0)

Test Results:

Access Type	Source IP	Result
HTTP (80)	Allowed IP	✅ Success
HTTP (80)	Disallowed IP	❌ Access Denied
HTTPS (443)	Any IP	✅ Success
