HEAD
# aws-ec2-s3-static-websit

# 📦 Hosting a Static Website with EC2 and S3

## 📥 Clone the Repository

```bash
git clone https://github.com/atulkamble/aws-ec2-s3-static-website.git
cd aws-ec2-s3-static-website
```

---

## 📌 Steps and Commands

### 🔹 1️⃣ Launch EC2 Instance and Connect via SSH

* **Launch EC2 Instance:** Use AWS Management Console to create a new EC2 instance.
* **Connect to EC2:**

```bash
ssh -i /path/to/your-key-pair.pem ec2-user@your-ec2-public-dns
```

---

### 🔹 2️⃣ Install Web Server on EC2

```bash
sudo yum update -y
sudo yum install git -y
git --version
git config --global user.name "your-username"
git config --global user.email "your-email"

sudo yum install httpd -y
sudo systemctl start httpd
sudo systemctl enable httpd
sudo systemctl status httpd

sudo usermod -a -G apache ec2-user
sudo chmod 755 /var/www/html

cd /var/www/html
sudo touch index.html
sudo nano index.html
```

---

### 🔹 3️⃣ Create an S3 Bucket

* **AWS Console:** Go to **S3** → **Create bucket** and follow the prompts.

---

### 🔹 4️⃣ Apply S3 Bucket Policy

* **Note your bucket ARN**, and replace `your-bucket-name` below.

**Bucket Policy:**

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::my-rick-roll/*"
        }
    ]
}```

> 💡 You can also use the [AWS Policy Generator](https://awspolicygen.s3.amazonaws.com/policygen.html) to create a custom policy.

---

### 🔹 5️⃣ Upload Image to S3

* In the AWS Console, navigate to your bucket.
* Click **Upload** and select `Coffee.img`.

---

### 🔹 6️⃣ Copy Image URL from S3

* After upload, go to the object details.
* Copy the **Object URL** (public URL of the image).

---

### 🔹 7️⃣ Add Image URL to `index.html`

**Sample `index.html`:**

```<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>S3 Hosted Video</title>
    <style>
        body {
            background-color: #121212;
            color: #ffffff;
            font-family: Arial, sans-serif;
            text-align: center;
            padding: 50px;
        }
        h1 {
            color: #00cfff;
        }
        video {
            width: 80%;
            max-width: 720px;
            border: 4px solid #00cfff;
            border-radius: 10px;
            box-shadow: 0 0 20px #00cfff66;
        }
    </style>
</head>
<body>
    <h1>#GOTCHA#</h1>
    <p>YOU HAVE BEEN RICK ROLLED</p>

    <video controls>
        <source src="https://my-rick-roll.s3.ap-south-1.amazonaws.com/rick-roll-video.mp4" type="video/mp4">
        Your browser does not support the video tag.
    </video>
</body>
</html>
```

> 📌 Replace the `src` URL with your copied S3 image URL.

---

### 🔹 8️⃣ Access Your Website via EC2 Public IP

* Find your **EC2 Public DNS/IP**
* Open your browser and go to:

```
http://your-ec2-public-dns
```

---

## ✅ Summary

This guide walks you through:

* Setting up an EC2 instance with Apache web server
* Hosting a static HTML page
* Storing and serving an image from an Amazon S3 bucket
* Integrating the image into your website via its public S3 URL
b90e3af (Updated index.html before pulling)
