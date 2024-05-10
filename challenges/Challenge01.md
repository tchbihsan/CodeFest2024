# Challenge 1: Deploy your first AWS resources via Terraform

**[Home](../README.md)** - [Next Challenge &gt;](./Challenge-02.md)

## Introduction

In this challenge, you will create a S3 bucket and a single NoSQL database using S3 and DynamoDB to create AWS resources that will be used in the upcoming challenges. 
  - Amazon S3 is an object storage service that is widely used for storing images, videos, files, hosting static website and many more features!
  - DynamoDB is designed to handle large volumes of data and suitable for applications that require low-latency access to large datasets like mobile apps, gaming, IoT and real-time analytics. 


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

Note: 
- The Terraform Plan/Apply workflow has been setup and you can look for the files at ./github/workflows directory.
- The Terraform workflows are currently on dispatch mode - this mean, you would need to trigger it manually from Github Actions console.

Try this via AWS Console:

1. Create a s3 bucket resource.
2. Then, create s3 bucket policy that includes the permission below and attach it to the s3 bucket:
   - s3:GetObject
   - s3:PutObject
   - s3:ListBucket
   - s3:DeleteObject
3. Next, create the resource to upload the html files below into the s3 bucket. These files can be found in the **html** directory.
   - index.html
   - error.html
4. Proceed to disable the public access block in your s3 bucket.
5. Lastly, enable the static web hosting option for your s3 bucket.
6. Now, create another s3 bucket via Terraform instead.

   
`Full answer below. To remove some parts for participants to figure out themselves`

```
resource "aws_s3_bucket" "xomcodefest_team0X" {
  bucket = "xomcodefest_team0X"
}

resource "aws_s3_bucket_public_access_block" "public_access" {
  bucket = aws_s3_bucket.xomcodefest_team0X.id
  block_public_acls = false
  block_public_policy = false
  ignore_public_acls = false
  restrict_public_buckets = false
}

resource "aws_s3_bucket_website_configuration" "static_hosting" {
  bucket = aws_s3_bucket.xomcodefest_team0X.id
  index_document {
    suffix = "index.html"
  } 
  error_document {
    key = "error.html"
  }
  
}

resource "aws_s3_object" "odefest_objects" {
  for_each = fileset("${path.module}/html", "*.html")
  bucket = aws_s3_bucket.xomcodefest_team0X.id
  key = each.value
  source = "html/${each.value}"
  content_type = "text/html"
}

resource "aws_s3_bucket_policy" "read_write" {
  bucket = aws_s3_bucket.xomcodefest_team0X.id
  policy = data.aws_iam_policy_document.codefest_bucket_policy.json
}

data "aws_iam_policy_document" "coedfest_bucket_policy" {
  statement {
    actions = [
      "s3:GetObject",
      "s3:PutObject",
      "s3:ListBucket",
      "s3:DeleteObject",
    ]
    resources = [
      var.xomcodefest_team0X-arn,
      "${var.xomcodefest_team0X-arn}/*",
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

Try this via AWS Console:

1. Create a DynamoDB table.
2. Then, write some test data into the table.
3. Now, try this via Terraform instead.

`Full answer below. To remove some parts for participants to figure out themselves`

```
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
    ttl {
    attribute_name = "TimeToExist"
    enabled        = false
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

1. You have successfully created a S3 bucket and attached a bucket policy to control access to your resources.
2. You have successfully created 1 DynamoDB and added some test data in your database. 
3. BONUS:
   - S3: Enable encryption for the bucket.
   - DynamoDB: Add TTL and enable encryption using custom key to the database.
     
   `Full answer below. To remove some parts for participants to figure out themselves`

```
Bonus Challenge #1:

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

```
Bonus Challenge #2:

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
* [S3 in Terraform](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket)
* [DynamoDB in Terraform](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/dynamodb_table)
* [What is Prinicipals in AWS?](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_principal.html)

