# Challenge 1: Deploy your first AWS resources via Terraform

**[Home](../README.md)** - [Next Challenge &gt;](./Challenge-02.md)

## Introduction

In this challenge, you will create a S3 bucket and a single NoSQL database using S3 and DynamoDB to create AWS resources that will be used in the upcoming challenges. 
  - Amazon S3 is an object storage service that is widely used for storing images, videos, files, hosting static website and many more features!
  - DynamoDB is designed to handle large volumes of data and suitable for applications that require low-latency access to large datasets like mobile apps, gaming, IoT and real time analytics. 

## Description

1. You need to deploy one S3 bucket and create a directory to store the html files.
2. You need to deploy one DynamoDB and write some test data into the newly created database.

## Key Concepts

- **Object storage service –** A service that manages data as object where each object contains data, metadata and a unique identifier.
- **NoSQL database -** DB system that:
  - differs from the traditional relational database
  - designed to handle large volumes of unstructured or semi-structured data
  - added flexibility in data modeling

## Implementation

Terraform:

1. Create a s3 bucket resource via Terraform.
2. Then, create s3 bucket policy that includes the permission below and attach it to the s3 bucket:
   - s3:GetObject
   - s3:PutObject
   - s3:ListBucket
   - s3:DeleteObject
3. Now, create the resource to upload the html files below into the s3 bucket.
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
2. You need to deploy one DynamoDB and write some test data into the newly created database.

  `Full code added for now. Will be updated before event.`
   
```Terraform code for S3
resource "aws_dynamodb_table" "resource-name" {
    name           = "resource-name"
    billing_mode   = "PAY_PER_REQUEST"
    hash_key       = "id"
    range_key      = "range"
    attribute {
        name = "id"
        type = "S"
    }
    attribute {
        name = "range"
        type = "S"
    }
    tags = {
        Name = "resource-name"
    }
  
}

resource "aws_dynamodb_table_item" "example" {
  table_name = aws_dynamodb_table.resource-name.name
  hash_key   = aws_dynamodb_table.resource-name.hash_key

  item = <<ITEM
{
  "exampleHashKey": {"S": "something"},
  "one": {"N": "11111"},
  "two": {"N": "22222"},
  "three": {"N": "33333"},
  "four": {"N": "44444"}
}
ITEM
}

```
   
## Success Criteria

1. You have successfully created a S3 bucket and attached a bucket policy to control access to your resources via Terraform.
2. You have successfully created 1 DynamoDB and added some test data in you database via Terraform.
3. BONUS:
   - S3: Enable encryption for the bucket.
   - DynamoDB: Add TTL and enable encryption using custom key to the database.
     
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

  DynamoDB:
  ttl {
    attribute_name = "TimeToExist"
    enabled        = false
  }

  server_side_encryption {
   enabled = true 
   // true + key arn -> use custom key
  }


```

## Learning Resources

* [Overview of Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingBucket.html)
* [Overview of Amazon DynamoDB](https://docs.aws.amazon.com/prescriptive-guidance/latest/modernization-rdbms-dynamodb/overview.html)
* [Providers in Terraform](https://registry.terraform.io/providers/hashicorp/aws/latest)
* [Variables in Terraform](https://developer.hashicorp.com/terraform/language/values/variables)

