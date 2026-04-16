

## 🔷 What is a Static Website?

A **static website** consists only of frontend files:
- HTML for content
- CSS for styling
- JS for interactivity
- Images, videos, etc.
No backend/server-side processing is involved.


## 🔧 Step-by-Step Guide to Host a Static Website on S3

---

### 🔹 Step 1: Create an S3 Bucket

1. Go to the S3 Console.
2. Click **"Create bucket"**.
3. Enter a **unique bucket name** (e.g., `my-website-suraj`).
4. Choose the **AWS Region**.
5. **Uncheck**: _"Block all public access"_
    - ⚠️ Required so your website is publicly accessible.
6. Acknowledge the warning and click **Create bucket**.

---

### 🔹 Step 2: Upload Your Website Files

1. Click on the bucket name you just created.
2. Go to the **"Objects"** tab.
3. Click **Upload** → Add files → Select `index.html`, `style.css`, etc.
4. Click **Upload**.

---

### 🔹 Step 3: Enable Static Website Hosting

1. Inside the bucket, go to the **"Properties"** tab.
2. Scroll down to **"Static website hosting"**.
3. Click **Edit**.
4. Enable it and provide:
    - **Index document**: `index.html`
    - (Optional) **Error document**: `error.html`
5. Click **Save changes**.
6. You’ll now see a **Bucket website endpoint** like:

```
http://my-website-suraj.s3-website.ap-south-1.amazonaws.com
```

---

### 🔹 Step 4: Make Files Public

By default, S3 objects are private. You must allow public access to serve them via HTTP.

#### Option 1: Set Bucket Policy (Recommended)

1. Go to the **Permissions** tab.
2. Scroll to **Bucket Policy** → Click **Edit**.
3. Paste the following JSON, replacing `YOUR_BUCKET_NAME`:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::YOUR_BUCKET_NAME/*"
    }
  ]
}
```

4. Save changes.
    

#### Option 2: Make each file public manually

- Go to each object → Click "Actions" → "Make public".  
    _(Tedious if you have many files)_
    

---

### 🔹 Step 5: Access Your Website

Now visit the **S3 static website endpoint URL**:

```
http://my-website-suraj.s3-website.ap-south-1.amazonaws.com
```

You should see your website live!