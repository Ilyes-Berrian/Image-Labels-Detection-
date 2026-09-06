# AWS S3 Image Detection with Lambda

This project uses **S3 → Lambda → Rekognition** to detect objects in uploaded images.

## Architecture

```text
S3 upload
   ↓
Lambda
   ↓
Rekognition
   ↓
Detected labels
```

## Requirements

* AWS account
* S3 bucket
* Lambda
* IAM role
* Python 3.x

## Setup

### 1. Create S3 Bucket

Create an S3 bucket and note its name.

Upload an image such as:

```text
test.jpg
```

### 2. Create IAM Role for Lambda

Create a Lambda execution role with:

```text
AWSLambdaBasicExecutionRole
```

and these permissions:

```json
{
  "Effect": "Allow",
  "Action": [
    "s3:GetObject"
  ],
  "Resource": "arn:aws:s3:::YOUR-BUCKET-NAME/*"
}
```

```json
{
  "Effect": "Allow",
  "Action": [
    "rekognition:DetectLabels"
  ],
  "Resource": "*"
}
```

### 3. Create Lambda

Create a Lambda function using **Python 3.x**.

Use `lambda_function.py` as the function code.

No AWS credentials are required in the code; Lambda uses its IAM execution role.

### 4. Add S3 Trigger

In Lambda:

```text
Add trigger → S3
```

Select your bucket and use:

```text
Event type: All object create events
```

### 5. Test

Upload an image to the bucket.

Then check:

```text
Lambda → Monitor → CloudWatch Logs
```

You should see:

```text
Detected labels:
Person
Car
Outdoor
...
```

## Important

Lambda and S3 must be configured in compatible AWS Regions.

Do **not** commit AWS access keys or secrets to GitHub.

## File Structure

```text
.
├── lambda_function.py
└── README.md
```
