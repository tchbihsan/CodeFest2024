# Challenge 1: Deploy your first AWS resources via Terraform

**[Home](../README.md)** - [Next Challenge &gt;](./Challenge-02.md)

## Introduction

In this challenge, you will create a S3 bucket using Amazon S3. This AWS service is an object storage service that is widely used for storing images, videos, files, hosting static website and many more features!

## Description

1. You need to deploy one S3 bucket and create a directory to store the html files.

## Key Concepts

- **Object storage service –** A service that manages data as object where each object contains data, metadata and a unique identifier.

## Implementation

Pre-req:
- Open Vs code
- create tfe files by adding .tfe extension
- optional : install terraform plugins (insert name)
- tfe plan , tfe apply (short recap)
- tfe providers and variables
- iam policy in aws 

Terraform:

1. Create a s3 bucket resource.
2. Then, create s3 bucket policy that includes the permission below and attach it to the s3 bucket:
   - s3:GetObject
   - s3:PutObject
   - s3:ListBucket
   - s3:DeleteObject
3. Now, create the resource to upload the html files below into the s3 bucket. These files can be found in the **html** directory.
   - index.html
   - error.html
4. Proceed to disable the public access block in your s3 bucket.
5. Lastly, enable the static web hosting option for your s3 bucket.

   
`Full answer below. To remove some parts for participants to figure out themselves`

```
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
  bucket = aws_s3_bucket.bucket-name.id
  index_document {
    suffix = "index.html"
  } 
  error_document {
    key = "error.html"
  }
  
}

resource "aws_s3_object" "resource-name" {
  for_each = fileset("${path.module}/html", "*.html")
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
2. BONUS:
   - S3: Enable encryption for the bucket.
     
   `Full answer below. To remove some parts for participants to figure out themselves`

```Terraform

  KMS key:

  resource "aws_kms_key" "mykey" {
    description             = "This key is used to encrypt bucket objects and dynamodb"
    deletion_window_in_days = 5
  }

  S3:
  resource "aws_s3_bucket_server_side_encryption_configuration" "resource-name" {
    bucket = aws_s3_bucket.bucket-name.id
  
    rule {
      apply_server_side_encryption_by_default {
        kms_master_key_id = aws_kms_key.mykey.arn
        sse_algorithm     = "aws:kms"
      }
    }
  }
```

## Learning Resources

* [Overview of Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingBucket.html)
* [Providers in Terraform](https://registry.terraform.io/providers/hashicorp/aws/latest)
* [Variables in Terraform](https://developer.hashicorp.com/terraform/language/values/variables)
* [What is Prinicipals in AWS?](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_principal.html

