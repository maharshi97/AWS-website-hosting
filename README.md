# AWS-website-hosting-Demo
# AWS Static Website Deployment Demo  This project contains a basic static website built with HTML, CSS, and JavaScript. The goal of this repository is to demonstrate how to deploy a static website on **Amazon Web Services (AWS)** using **S3** and optionally **CloudFront** for global distribution.  ## 📁 Project Structure


# 🚀 AWS Static Website Deployment Demo

This project is a basic static website built using HTML, CSS, and JavaScript. It demonstrates how to deploy a static website to **Amazon Web Services (AWS)** using multiple methods:

1. [S3 Static Website Hosting](#-method-1-deploying-on-amazon-s3)
2. [S3 + CloudFront CDN (HTTPS)](#-method-2-deploy-with-s3--cloudfront-for-httpscdn)
3. [EC2 Instance with Apache Web Server](#-method-3-deploying-on-ec2-with-apache)

---

## 📁 Project Structure

```
AWSDemo/
├── images/
├── index.html
├── styles.css
└── script.js
```

---

## 🔧 Method 1: Deploying on Amazon S3

> Host a static site directly from an S3 bucket

### Step 1: Create an S3 Bucket

1. Go to [AWS S3 Console](https://s3.console.aws.amazon.com/s3/)
2. Click **Create bucket**
3. Enter a unique name (e.g., `my-static-site-demo`)
4. **Uncheck** “Block all public access”
5. Acknowledge the warning and click **Create bucket**

### Step 2: Enable Static Website Hosting

1. Go to your bucket → **Properties**
2. Scroll to **Static website hosting** → click **Edit**
3. Choose “Enable”
4. Set `index.html` as the index document
5. Save changes

### Step 3: Upload Files

1. Go to the **Objects** tab → click **Upload**
2. Upload all files from the `AWSDemo/` folder
3. After uploading, select files → **Actions** → **Make public**

### Step 4: Access the Website

1. Go to **Properties** → find the **Static website hosting endpoint**
2. Open the endpoint URL in your browser

---

## 🌍 Method 2: Deploy with S3 + CloudFront (for HTTPS/CDN)

> Add CDN and SSL for better speed and security

### Step 1: Complete S3 Setup (From Method 1)

Ensure your static site is already live via S3.

### Step 2: Create a CloudFront Distribution

1. Go to [CloudFront Console](https://console.aws.amazon.com/cloudfront/)
2. Click **Create Distribution**
3. For **Origin domain**, use your S3 static hosting endpoint (not bucket name!)
4. Set **Viewer Protocol Policy**: Redirect HTTP to HTTPS
5. Set **Default root object**: `index.html`
6. Click **Create Distribution**

### Step 3 (Optional): Configure Custom Domain + SSL

1. Request a certificate in **AWS Certificate Manager (ACM)**
2. Point your domain to CloudFront using Route 53 or other DNS provider

---

## 🖥️ Method 3: Deploying on EC2 with Apache

> Full control via a virtual server running Apache

### Step 1: Launch an EC2 Instance

1. Go to [EC2 Console](https://console.aws.amazon.com/ec2/)
2. Click **Launch Instance**
3. Choose:
   - **Amazon Linux 2** or **Ubuntu**
   - Instance type: `t2.micro` (Free Tier)
4. Configure a security group:
   - Allow **SSH (22)** and **HTTP (80)** traffic
5. Launch with a key pair (`.pem` file)

### Step 2: Connect via SSH

```bash
ssh -i /path/to/your-key.pem ec2-user@<EC2_PUBLIC_IP>
```
(For Ubuntu use `ubuntu@<EC2_PUBLIC_IP>`)

### Step 3: Install Apache

#### For Amazon Linux 2:

```bash
sudo yum update -y
sudo yum install httpd -y
sudo systemctl start httpd
sudo systemctl enable httpd
```

#### For Ubuntu:

```bash
sudo apt update
sudo apt install apache2 -y
sudo systemctl start apache2
sudo systemctl enable apache2
```

### Step 4: Upload Website Files

#### Option A: From Local via SCP

```bash
scp -i /path/to/your-key.pem -r ./AWSDemo/* ec2-user@<EC2_PUBLIC_IP>:/tmp/
```

Then:

```bash
sudo mv /tmp/* /var/www/html/
```

#### Option B: Clone from GitHub on EC2

```bash
sudo yum install git -y   # or `sudo apt install git` for Ubuntu
git clone https://github.com/varunbgit/AWSDemo.git
sudo cp -r AWSDemo/* /var/www/html/
```

### Step 5: View the Site

- Open a browser and go to: `http://<EC2_PUBLIC_IP>`

---

## ✅ Best Practices

- Enable versioning on S3
- Set `Cache-Control` headers for performance
- Use HTTPS via ACM or Let’s Encrypt
- Monitor EC2 using CloudWatch
- Use CloudFront for EC2 if targeting a global audience

---

## 🧽 Cleanup

To avoid charges:

- **S3**: Delete bucket
- **CloudFront**: Disable and delete distribution
- **EC2**: Stop or terminate the instance
- **DNS/ACM**: Remove unused domains/certs

---

## 📞 Support

For issues or improvements, feel free to [create an issue](https://github.com/varunbgit/AWSDemo/issues) or contact [Varun B](https://github.com/varunbgit).

---

## 📄 License

This project is licensed under the MIT License.
