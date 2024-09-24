### **Step-by-Step Lab: Hosting a Static Website on Amazon S3**

In this lab, we will walk through the process of hosting a static website using **Amazon S3**. A static website delivers content, such as HTML, CSS, and images, to users without the need for dynamic backend processing (e.g., databases or server-side scripting).

---

### **Objectives**

By the end of this lab, you will:
- Create an S3 bucket to store static website files.
- Upload HTML and image files to the S3 bucket.
- Enable static website hosting in Amazon S3.
- Configure public access to the bucket using an S3 bucket policy.
- Test the hosted static website.

---

### **Prerequisites**

- **AWS Account**: Ensure that you have access to an AWS account.
- **IAM Permissions**: You should have sufficient permissions to create S3 buckets, upload files, and manage bucket policies.

---

### **Step 1: Create an S3 Bucket**

1. **Log in to the AWS Management Console**.
2. Navigate to the **S3 Dashboard**.
3. Click on **Create Bucket**.
4. In the **Create Bucket** dialog:
   - **Bucket name**: Provide a unique bucket name (e.g., `my-static-website-bucket`).
   - **Region**: Select the region where you want the bucket to reside.
   - Leave all other options at their defaults (e.g., Block Public Access should remain enabled for now).
5. Click **Create Bucket**.

---

### **Step 2: Upload Files to the S3 Bucket**

Now that the bucket is created, upload your website files (HTML, CSS, images, etc.).

1. Navigate to your newly created **S3 bucket**.
2. Click on **Upload** and choose **Add Files**.
3. Select your **website files** from your local machine:
   - For example, upload the following files:
     - `index.html` (your homepage).
     - `coffee.jpg` (an image file).
     - `beach.jpg` (another image file).
4. After selecting the files, click **Upload**.

---

### **Step 3: Enable Static Website Hosting**

1. After uploading the files, click on the **Properties** tab for your S3 bucket.
2. Scroll down to the **Static Website Hosting** section.
3. Click on **Edit**.
4. Select **Enable** static website hosting.
5. Under **Hosting type**, select **Host a static website**.
6. In the **Index document** field, enter the name of your homepage file (e.g., `index.html`).
7. Click **Save**.
8. After saving, you will see the **Bucket Website Endpoint** URL at the bottom of the **Static Website Hosting** section. This is the URL where your static website will be accessible.

---

### **Step 4: Configure Public Access to the S3 Bucket**

To make the website files accessible to the public, we need to configure the bucket policy to allow public reads.

1. Go to the **Permissions** tab of your bucket.
2. Scroll down to the **Block Public Access** section and click **Edit**.
3. **Uncheck all the boxes** to disable the blocking of public access. Be careful to do this only if you are certain the bucket should be publicly accessible.
4. Confirm the changes by typing **"confirm"** in the prompt and click **Save**.

---

### **Step 5: Create a Bucket Policy to Allow Public Access**

Now that public access is allowed, we will attach a bucket policy to make the objects publicly readable.

1. Scroll down to the **Bucket Policy** section under the **Permissions** tab and click **Edit**.
2. Copy and paste the following **Bucket Policy** into the editor:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::my-static-website-bucket/*"
    }
  ]
}
```

- Replace `my-static-website-bucket` with the actual name of your bucket.
- This policy allows public read access to all objects in the bucket.

3. Click **Save Changes** to apply the policy.

---

### **Step 6: Test the Static Website**

1. Go back to the **Properties** tab of your S3 bucket.
2. Scroll down to the **Static Website Hosting** section and copy the **Bucket Website Endpoint** URL.
3. Paste the **URL** into your web browser to access your website. For example, the URL may look like this:
   ```
   http://my-static-website-bucket.s3-website-us-east-1.amazonaws.com
   ```
4. You should see the content of your `index.html` file displayed as the homepage.

5. To view images (e.g., `coffee.jpg` or `beach.jpg`), you can append the image filename to the bucket website URL. For example:
   ```
   http://my-static-website-bucket.s3-website-us-east-1.amazonaws.com/coffee.jpg
   ```
   This URL will display the `coffee.jpg` image.

---

### **Step 7: Additional Configurations (Optional)**

#### **Setting a Custom Error Page**
You can configure a custom error page (e.g., `error.html`) to be shown if a user accesses a non-existent page.

1. In the **Static Website Hosting** settings (under **Properties** tab), set the **Error document** field to the name of your error page (e.g., `error.html`).
2. Upload the `error.html` file to your S3 bucket.

---

### **Summary**

Congratulations! You have successfully hosted a static website on Amazon S3. You learned how to:
- Create an S3 bucket for website hosting.
- Upload HTML and image files to the bucket.
- Enable static website hosting.
- Configure public access to the bucket using a bucket policy.
- Access the website and verify that it is publicly accessible.

By following these steps, you can host any static website using Amazon S3, taking advantage of its scalability, reliability, and simplicity.

Let me know if you need further assistance or more advanced configurations!



