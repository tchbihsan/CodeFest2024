# Challenge 1: Deploy your first AWS resources via Terraform

**[Home](../README.md)** - [Next Challenge &gt;](./Challenge-02.md)

## Introduction

In this challenge, you will create a S3 bucket and a single NoSQL database using S3 and DynamoDB to create AWS resources that will be used in the upcoming challenges. 
  - Amazon S3 is an object storage service that is widely used for storing images, videos, files, hosting static website and many more features!
  - DynamoDB is designed to handle large volumes of data and suitable for applications that require low-latency access to large datasets like mobile apps, gaming, IoT and real time analytics. 

## Key Concepts

- **Object storage service –** A service that manages data as object where each object contains data, metadata and a unique identifier.
- **NoSQL database -** DB system that:
  - differs from the traditional relational database
  - designed to handle large volumes of unstructured or semi-structured data
  - added flexibility in data modeling

## Description

* You need to deploy 1 S3 bucket and 1 DynamoDB in the **us-east-2** region. 
  `Full code added for now. Will be updated before event.`

```Terraform code for S3
resource "aws_s3_bucket" "resource-name" {
  bucket = "bucket-name"
}

resource "aws_s3_bucket_public_access_block" "resource-name" {
  bucket = aws_s3_bucket.bucket-name.id
  block_public_acls = false
  block_public_policy = false
  ignore_public_acls = false
  restrict_public_buckets = false
}

resource "aws_s3_bucket_website_configuration" "resource-name" {
  bucket = aws_s3_bucket.bucekt-name.id
  index_document {
    suffix = "index.html"
  } 
  error_document {
    key = "error.html"
  }
  
}

resource "aws_s3_object" "resource-name" {
  for_each = fileset("${path.module}/", "*.html")
  bucket = aws_s3_bucket.bucket-name.id
  key = each.value
  source = "html/${each.value}"
  content_type = "text/html"
}

resource "aws_s3_bucket_policy" "resource-name" {
  bucket = aws_s3_bucket.bucket-name.id
  policy = data.aws_iam_policy_document.s3_bucket_policy.json
}

data "aws_iam_policy_document" "s3_bucket_policy" {
  statement {
    actions = [
      "s3:GetObject",
      "s3:PutObject",
      "s3:ListBucket",
      "s3:DeleteObject",
    ]
    resources = [
      var.bucket-arn,
      "${var.bucket-arn}/*",
    ]
    principals {
      type = "AWS"
      identifiers = [
        "*"
      ]
    }
  }
}
}
```

## Success Criteria

1. You have successfully created a S3 bucket and attached a bucket policy to control access to your resources via Terraform.
2. You have successfully created 1 DynamoDB and added some test data in you database via Terraform.
3. BONUS:WIP
   `Full answer below. To remove some parts for participants to figure out themselves`

```Terraform



```

## Learning Resources

* [Overview of Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingBucket.html)
