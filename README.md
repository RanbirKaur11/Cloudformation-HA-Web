# 🌐 CloudFormation – Highly Available Web App (Starter)

This repository contains a starter **AWS CloudFormation** template (`HA-Web.yaml`) to provision the **networking foundation** for a highly available web application:

- ✅ **Custom VPC** across 2 Availability Zones  
- ✅ **Public subnets**, route table, and Internet Gateway  
- ✅ **Application Load Balancer (ALB)** security group and ALB (if included in your version)  

> Use this as a base to add a Launch Template, Auto Scaling Group, Target Group/Listener, and (optionally) RDS.

---

## 📦 Files
- `HA-Web.yaml` – Main CloudFormation template

---

## ✅ Prerequisites

- An AWS account with permissions for VPC/EC2/ELB/CloudFormation  
- AWS CLI configured:  
  ```bash
  aws sts get-caller-identity


(Optional) An EC2 Key Pair if your template includes EC2 instances

🚀 Deploy

You can deploy from AWS CloudShell or your local machine.

# From the repo root
aws cloudformation deploy \
  --template-file HA-Web.yaml \
  --stack-name HighAvailableAppStack \
  --capabilities CAPABILITY_NAMED_IAM \
  --region us-east-1

✅ Check Stack Status
aws cloudformation describe-stacks \
  --stack-name HighAvailableAppStack \
  --query "Stacks[0].StackStatus"

✅ View Outputs (e.g., ALB DNS if present)
aws cloudformation describe-stacks \
  --stack-name HighAvailableAppStack \
  --query "Stacks[0].Outputs"

🔧 Parameters (Example)

Your template may include these defaults — customize as needed:

VpcCIDR: 10.0.0.0/16

PublicSubnet1CIDR: 10.0.1.0/24

PublicSubnet2CIDR: 10.0.2.0/24

PrivateSubnet1CIDR: 10.0.11.0/24

PrivateSubnet2CIDR: 10.0.12.0/24

KeyPairName: Existing EC2 key pair name (if EC2 used)

Example with parameter override:

aws cloudformation deploy \
  --template-file HA-Web.yaml \
  --stack-name HighAvailableAppStack \
  --parameter-overrides KeyPairName=MyKeyPair \
  --capabilities CAPABILITY_NAMED_IAM

🧹 Cleanup

Delete the stack to remove provisioned resources:

aws cloudformation delete-stack --stack-name HighAvailableAppStack
aws cloudformation wait stack-delete-complete --stack-name HighAvailableAppStack


💡 If you later add S3 buckets with data, consider using DeletionPolicy: Retain to avoid accidental data loss.

🛠 Troubleshooting

❌ CREATE_FAILED (Name Already Exists)
S3 bucket names are global. Use a unique name or let CloudFormation auto-name by omitting BucketName.

❌ Stack “Drifted”
If something was deleted outside CloudFormation, detect and fix:

aws cloudformation detect-stack-drift --stack-name HighAvailableAppStack


Then update the stack to recreate missing resources.

❌ Permissions Issues
If aws s3 ls shows nothing, you might lack s3:ListAllMyBuckets.
Check a single bucket instead:

aws s3api head-bucket --bucket <name>

▶️ Next Steps (Recommended)

Add:

Launch Template + Auto Scaling Group

Target Group + Listener for the ALB

UserData (install Nginx / sample app)

(Optional) RDS (Multi-AZ) for the data tier

Add Outputs: ALB DNS, Security Group IDs, VPC/Subnet IDs

Add Tags for cost allocation and organization

🔒 .gitignore (Optional)

This repository already includes a `.gitignore` file to keep it clean and professional.  
It prevents tracking of:

- VS Code settings (`.vscode/`)
- Temporary logs (`*.log`, `*.tmp`, `~*`)
- Microsoft Office backup files (`~$*.docx`, `*.xlsx`, etc.)
- OS-generated files (`.DS_Store`, `Thumbs.db`)

This ensures that only relevant project files (like `HA-Web.yaml`) are committed.


---

### ✅ Next Step
1. Open **VS Code** → create a new file → name it `README.md`.  
2. Paste the entire content above.  
3. Save it.  
4. Run these in Git Bash:

```bash
git add README.md
git commit -m "Add README with deployment guide"
git push


When you refresh your repo on GitHub, it will look beautifully formatted.


