# Minio-Setup-Locally

# **Local MinIO Setup (Windows)**

This project uses **MinIO** as a local S3-compatible storage for testing and development.

---

## **Step 1 — Create Data Folder**

Open **CMD** and run:

```cmd
mkdir "C:\Users\Alvin Kayi\Desktop\minio-data"
```

This folder will persist your files even if the container is stopped or restarted.

---

## **Step 2 — Run MinIO Docker Container**

Run the following one-liner in **CMD**:

```cmd
docker run -d --name minio -p 9000:9000 -p 9001:9001 -e MINIO_ROOT_USER=admin -e MINIO_ROOT_PASSWORD=admin123 -v "C:\Users\Alvin Kayi\Desktop\minio-data:/data" minio/minio server /data --console-address ":9001"
```

* `9000` → S3 API endpoint
* `9001` → MinIO Web Console
* `admin` / `admin123` → Access keys for S3 API

---

## **Step 3 — Access the Web Console**

Open your browser and go to:

```
http://localhost:9001
```

Login with:

* **Username:** `admin`
* **Password:** `admin123`

You can now create buckets, upload files, and manage your local S3 storage.

---

## **Step 4 — Test S3 API with Python**

Install `boto3` if you don’t have it:

```cmd
pip install boto3
```

Then run:

```python
import boto3

# Connect to local MinIO
s3 = boto3.client(
    "s3",
    endpoint_url="http://localhost:9000",
    aws_access_key_id="admin",
    aws_secret_access_key="admin123"
)

# Create a test bucket
s3.create_bucket(Bucket="testbucket")

# Upload a file
s3.put_object(Bucket="testbucket", Key="hello.txt", Body=b"Hello MinIO!")

# List objects in bucket
objects = s3.list_objects(Bucket="testbucket")
print(objects)
```

---

## **Notes**

* MinIO is **lightweight** and works on Windows without any Linux emulation.
* Data persists in `C:\Users\Alvin Kayi\Desktop\minio-data`.
* Fully **S3-compatible**, so you can use it with Python, FastAPI, Node.js, or any S3 library.
* For production, use proper credentials and secure the Web Console.

---


Do you want me to do that?
