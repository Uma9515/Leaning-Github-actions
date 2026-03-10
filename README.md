# 🚀 AWS Static Website POC

Deploy a static website to AWS S3 + CloudFront automatically using Terraform and GitHub Actions.

---

## 📁 Project Structure

```
your-repo/
├── index.html                        ← Your website
├── terraform/
│   ├── main.tf                       ← AWS resources (S3 + CloudFront)
│   ├── variables.tf                  ← Configuration variables
│   └── outputs.tf                    ← Prints your website URL
└── .github/
    └── workflows/
        └── deploy.yml                ← Auto-deploy pipeline
```

---

## ⚙️ One-Time Setup (do this before first push)

### Step 1 – Get your AWS credentials

1. Log in to your **AWS sandbox console**
2. Go to **IAM → Users → your user → Security credentials**
3. Click **"Create access key"**
4. Copy the **Access Key ID** and **Secret Access Key**

> ⚠️ Save these – you won't see the secret key again!

---

### Step 2 – Add credentials to GitHub

1. Open your GitHub repo
2. Go to **Settings → Secrets and variables → Actions**
3. Click **"New repository secret"** and add these two:

| Secret Name             | Value                          |
|------------------------|-------------------------------|
| `AWS_ACCESS_KEY_ID`     | Your Access Key ID from Step 1 |
| `AWS_SECRET_ACCESS_KEY` | Your Secret Access Key         |

---

### Step 3 – Push your code

```bash
# Clone your repo (or create a new one and copy these files in)
git clone https://github.com/YOUR_USERNAME/YOUR_REPO.git
cd YOUR_REPO

# Copy all POC files in, then:
git add .
git commit -m "Initial POC deployment"
git push origin main
```

---

## 🔄 What Happens Automatically

Every time you push to `main`:

```
Push to GitHub
    ↓
GitHub Actions triggers
    ↓
AWS credentials configured
    ↓
Terraform creates S3 bucket + CloudFront  (only on first run)
    ↓
index.html uploaded to S3
    ↓
CloudFront cache cleared
    ↓
✅ Website is LIVE!
```

---

## 🌐 Find Your Website URL

After the first successful deployment:

1. Go to your GitHub repo
2. Click **Actions** tab
3. Click the latest workflow run
4. Scroll to the **"Print website URL"** step
5. Copy the URL — it looks like:  
   `https://d1abc2xyz.cloudfront.net`

---

## ✏️ Making Changes

Just edit `index.html` and push:

```bash
# Make changes to index.html, then:
git add index.html
git commit -m "Update website content"
git push origin main
```

The pipeline runs automatically and your site updates in ~2 minutes!

---

## 🧹 Clean Up (avoid AWS charges)

When you're done with the POC, destroy all resources:

```bash
cd terraform
terraform init
terraform destroy -auto-approve
```

> This deletes the S3 bucket and CloudFront distribution.

---

## ❓ Troubleshooting

| Problem | Fix |
|---------|-----|
| Workflow fails with "credentials error" | Check GitHub Secrets are named exactly right |
| Website shows old version | Wait 2–3 min for CloudFront to refresh |
| `terraform init` fails | Check you have internet access and Terraform installed |
| Bucket name conflict | Edit `project_name` in `terraform/variables.tf` |

---

## 💡 What You Learned

- ✅ **Terraform** – defines AWS infrastructure as code
- ✅ **S3** – stores your website files
- ✅ **CloudFront** – serves your website globally via CDN (HTTPS!)
- ✅ **GitHub Actions** – automates the whole deploy on every push
