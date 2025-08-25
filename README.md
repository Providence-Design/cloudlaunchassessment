

````markdown
# CloudLaunch Static Website Deployment

This project demonstrates hosting a static website on **Amazon S3** and managing access through **IAM policies**. The setup ensures that a specific IAM user (`cloudlaunch-user`) can log in and view S3 resources securely.  



## 📌 Task 1: S3 Static Website Hosting

1. **Created an S3 bucket**:  
   - Name: `cloudlaunch-site-bucket-providence`  
   - Enabled static website hosting.  
   - Uploaded website files (HTML/CSS/JS).  

2. **Bucket Policy (Public Read Access)**:  
   The following policy was attached to the bucket to allow public read access for the website:  

   ```json
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "PublicReadGetObject",
               "Effect": "Allow",
               "Principal": "*",
               "Action": "s3:GetObject",
               "Resource": "arn:aws:s3:::cloudlaunch-site-bucket-providence/*"
           }
       ]
   }
````

3. **Static Website Endpoint**:

   ```
   http://cloudlaunch-site-bucket-providence.s3-website.us-east-2.amazonaws.com/
   ```


## 📌 Task 2: IAM User Setup

1. **Created IAM User**:

   * Username: `cloudlaunch-user`
   * Programmatic and console access enabled.
   * Enforced password reset on first login.

2. **IAM Policy Attached to User**:
   The following JSON policy was attached, allowing the user to view S3 resources (buckets and objects):

   ```json
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "ListAllMyBuckets",
               "Effect": "Allow",
               "Action": [
                   "s3:ListAllMyBuckets",
                   "s3:GetBucketLocation"
               ],
               "Resource": "*"
           },
           {
               "Sid": "AllowBucketAccess",
               "Effect": "Allow",
               "Action": [
                   "s3:ListBucket",
                   "s3:GetBucketLocation"
               ],
               "Resource": "arn:aws:s3:::cloudlaunch-site-bucket-providence"
           },
           {
               "Sid": "AllowObjectAccess",
               "Effect": "Allow",
               "Action": "s3:GetObject",
               "Resource": "arn:aws:s3:::cloudlaunch-site-bucket-providence/*"
           }
       ]
   }
   ```

3. **User Console Login Details**:

   * AWS Console URL:

     ```
     https://993907661290.signin.aws.amazon.com/console
     ```

     or, if account alias was set:

     ```
     https://<ACCOUNT_ALIAS>.signin.aws.amazon.com/console
     ```
   * Username: `cloudlaunch-user`
   * Password: `7BvVR8a_`  or ama@@23ama (temporary password provided during creation; user must change on first login).


## 📌 (Optional) CloudFront Distribution

If CloudFront was configured for improved performance and HTTPS:

* **CloudFront URL**:

  ```

## 📌 Task 3 (Optional): CloudFront Distribution

### STEP 1: Get Started

1. Go to the **AWS Management Console** → Search **CloudFront** → Click **CloudFront service**.
2. Click **“Create distribution”** → Choose **Web**.


### STEP 2: Specify Origin

1. **Origin Domain Name**: Enter your **S3 website endpoint** (not the bucket name).
   Example:

   ```
   cloudlaunch-site-bucket-providence.s3-website.us-east-2.amazonaws.com
   ```
2. **Origin Path**: Leave blank
3. **Enable Origin Shield**: No
4. **Connection Attempts**: 3
5. **Connection Timeout**: 10

> ⚠️ Make sure you are using the **S3 website endpoint**, not the standard S3 bucket ARN.



### STEP 3: Enable Security

1. **Security Protections**: None (default)
2. **Use Monitor Mode**: No
3. **Use Existing WAF Configuration**: No


### STEP 4: Review and Create

1. Review all settings
2. Click **Create Distribution**
3. Wait **15–20 minutes** for the status to change from **In Progress** → **Deployed**


### STEP 5: Get CloudFront URL

1. Click on your newly created distribution
2. Copy the **Domain Name** (it will look like `d1234567890abcdef.cloudfront.net`)
3. Test in a browser to confirm your website loads with HTTPS


```markdown
**Static Website URL (S3)**: 
http://cloudlaunch-site-bucket-providence.s3-website.us-east-2.amazonaws.com/

**CloudFront URL**:
https://d12qu1lxmknkv0.cloudfront.net/
```

## ✅ Expected Results

* The **S3 static site** is accessible publicly via the S3 website endpoint (and optionally CloudFront).
* The **IAM user** can log in to the AWS console, list the S3 buckets, and access the objects in the `cloudlaunch-site-bucket-providence` bucket.
* The IAM user **cannot** access other AWS services or resources outside of what was explicitly allowed.



## 🔐 Security Notes

* Public read access is limited to the website bucket only.
* IAM user credentials must be rotated regularly.
* Password reset was enforced on first login.

```
