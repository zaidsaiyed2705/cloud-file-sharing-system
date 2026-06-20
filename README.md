# Cloud-Based File Upload & Sharing System

A serverless file upload and sharing system built using AWS S3, Lambda, and API Gateway.
Files are uploaded directly from the browser to S3 using pre-signed URLs — no file ever
passes through a backend server.

## Architecture

Browser → API Gateway (GET /get-upload-url) → Lambda (generates pre-signed S3 URLs) → S3 (private bucket)

1. User selects a file in the browser.
2. Browser calls the API Gateway endpoint to request a pre-signed upload URL.
3. Lambda generates two pre-signed URLs: one for uploading (PUT), one for sharing (GET, expires in 1 hour).
4. Browser uploads the file directly to S3 using the upload URL.
5. The share URL is displayed for the user to copy and send.

## AWS Services Used
- **S3** — private bucket for file storage (Block Public Access enabled)
- **Lambda** (Python 3.12) — generates pre-signed URLs
- **API Gateway** (HTTP API) — exposes the Lambda function over HTTPS
- **IAM** — least-privilege role for Lambda (s3:PutObject, s3:GetObject scoped to one bucket only)

## Folder Structure
- `backend/lambda_function.py` — Lambda source code
- `frontend/index.html` — upload UI
- `screenshots/` — proof of working setup

## Steps Performed
1. Created a private S3 bucket with CORS enabled for PUT/GET.
2. Created an IAM role with an inline least-privilege policy for Lambda.
3. Created a Lambda function to generate pre-signed upload/download URLs.
4. Created an HTTP API in API Gateway integrated with the Lambda, with CORS enabled.
5. Built a simple HTML/JS frontend that requests a pre-signed URL and uploads directly to S3.
6. Tested end-to-end: uploaded a file, received a shareable link, verified download.

## Screenshots
See `/screenshots` folder:
1. S3 bucket with public access blocked
2. IAM inline policy (least privilege)
3. Lambda function deployed
4. API Gateway route + invoke URL
5. Upload form in browser
6. Successful upload with share link generated
7. Share link opened, file accessible
8. Uploaded object visible inside S3 bucket

## Security Notes
- Bucket is fully private; access only possible via time-limited pre-signed URLs.
- IAM role scoped to a single bucket, two actions only (no wildcard permissions).
- Upload URLs expire in 5 minutes, share URLs expire in 1 hour.

## Cleanup
After testing, the Lambda function, API Gateway, and S3 bucket objects were removed to avoid unnecessary cost.
