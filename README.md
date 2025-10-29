# Static-Website-Hosting
This repository is about hosting a Static website on S3

This project demonstrates how to securely host a static website using **Amazon S3**, **CloudFront**, and **Route 53** with HTTPS enabled via **AWS Certificate Manager (ACM)**. The website is stored in a private S3 bucket while CloudFront acts as a global CDN layer, providing fast, secure, and highly available delivery. A custom domain purchased from Route 53 is mapped to the CloudFront distribution, and visitor access logs are stored in another S3 bucket to track and analyze website traffic. This is a production-ready architecture that follows AWS best practices — fully secured, globally optimized, and analytics-ready.

## **Steps to Complete the Project**  

### **Step 1: Register a Custom domain in Route 53**
1. Sign in to your **AWS Management Console**.  
2. Navigate to **Route 53 Dashboard**.
3. Click on **Register a domain**.
4. Search for a domain and check its availability, I chose saikiranprojets.click
5. Select the domain and proceed to Checkout. It will take few minutes for registration.

### **Step 2: Create an S3 Bucket**
1. Navigate to **S3 Dashboard**.
2. Click on Create Bucket.
3. Configure the following:
    - **Bucket type:** General purpose
    - **Bucket name:** saikiranprojets.click (same as domain name)
    - **Object Ownership:** ACLs disabled (recommended)
    - Tick the box for **Block all public access**
    - **Bucket Versioning:** Enable
    - **Encryption Type:** Server-side encryption with Amazon S3 managed keys (SSE-S3)
    - **Bucket Key:** Enable
    - Click **Create Bucket**
4. Upload **index.html** file(and any CSS, JS, images etc.)
5. Do not make any other changes.

### **Step 3: Request an SSL Certificate using AWS Certificate Manager (ACM)**
To enable HTTPS on your domain, we need a public SSL/TLS certificate from AWS Certificate Manager (ACM).
  1. Go to **AWS Certificate Manager (ACM)**

     Important: Make sure you are in N. Virginia (us-east-1) region (CloudFront only accepts certificates from this region)
  2. Click on **Request a Certificate**
  3. Certificate type: **Request a public certificate**
  4. Domain name : saikiranprojets.click
  5. Allow export : **Disable export**
  6. Validation method : **DNS validation**
  7. Key algorithm : **RSA 2048**
  8. Click on **Request**
  9. Add DNS records to verify Ownership

      - AWS will show a CNAME record
      
      - Click “Create record in Route 53” (it adds it automatically)
      
      - Wait for status to change from "Pending validation" → "Issued"
  
  **Once the status shows “Issued”, your domain now has a valid HTTPS certificate ready to use in CloudFront.**

### **Step 4 : Create a Cloudfront Distribution**
  1. Distribution name : **saikiranprojets.click-cfd**
  2. Distribution type : **Single website or app**
  3. Domain : **saikiranprojets.click**
  4. Origin type : **Amazon S3**
  5. S3 origin : **saikiranprojets.click**
  6. Allow private S3 bucket access to CloudFront
  7. Leave everything else in their recommended settings
  8. Web Application Firewall : Do not enable security protections
  9. TLS Certificate : Add the Certificate that we received earlier
  10. Review and click on Create Distribution
  11. Go to CloudFront → Distributions --> Click your distribution --> Go to Settings tab --> Find Default Root Object --> Enter: index.html -->Click Save Changes

### **Step 5 : Route 53 DNS Setup**
Go to **Route 53** → **Hosted Zones** → saikiranprojets.click
1. Click on **Create Record**
2. Record name: leave it blank or add anything that comes in front of your domain (like blog, project, etc)
3. Turn on the **alias**
4. Route traffic to : **Alias to CloudFront distribution**
5. choose distribution : select the cloudfront distribution that we created in previous step
6. Click **create records**
7. Wait for it to go Live.

### **Step 6: Test the Website (Important)**
- Open your browser and visit: **https://saikiranprojets.click**
- You should see your `index.html` loading with a **secure HTTPS lock (🔒)**.
- If you see **403 AccessDenied**, it means the file was not uploaded **directly in the root of the S3 bucket** — fix that and reload.

**Done, The website is Live now !!**

### **Technologies Used**
- Amazon S3 (private origin with encryption & versioning)
- Amazon CloudFront (CDN + global edge caching)
- Route 53 (DNS + custom domain)
- AWS Certificate Manager (SSL/TLS for HTTPS)


**This setup follows AWS best practices — fully secure, globally optimized, cost-efficient, and production ready.**


## **Architecture Diagram**
<img width="1536" height="1024" alt="architecture" src="https://github.com/user-attachments/assets/215d7c67-4768-40f5-9598-97480f0c8993" />


## **Live Website Preview**
<img width="1919" height="1147" alt="output" src="https://github.com/user-attachments/assets/de050903-92aa-493a-9e17-e8d25f96dcc2" />
