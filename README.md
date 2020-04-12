# AWS S3 bucket Terraform module

Terraform module which creates S3 bucket on AWS with all (or almost all) features provided by Terraform AWS provider.

This type of resources are supported:

* [S3 Bucket](https://www.terraform.io/docs/providers/aws/r/s3_bucket.html)
* [S3 Bucket Policy](https://www.terraform.io/docs/providers/aws/r/s3_bucket_policy.html)

These features of S3 bucket configurations are supported:

- static web-site hosting
- access logging
- versioning
- CORS
- lifecycle rules
- server-side encryption
- object locking
- Cross-Region Replication (CRR)
- ELB log delivery bucket policy

## Terraform versions

Only Terraform 0.12 is supported.

## Usage

### Private bucket with versioning enabled

```hcl
module "s3_bucket" {
  source = "terraform-aws-modules/s3-bucket/aws"

  bucket = "my-s3-bucket"
  acl    = "private"

  versioning = {
    enabled = true
  }

}
```

### Bucket with ELB access log delivery policy attached

```hcl
module "s3_bucket_for_logs" {
  source = "terraform-aws-modules/s3-bucket/aws"

  bucket = "my-s3-bucket-for-logs"
  acl    = "log-delivery-write"

  # Allow deletion of non-empty bucket
  force_destroy = true

  attach_elb_log_delivery_policy = true
}
```

## Conditional creation

Sometimes you need to have a way to create S3 resources conditionally but Terraform does not allow to use `count` inside `module` block, so the solution is to specify argument `create_bucket`.

```hcl
# This S3 bucket will not be created
module "s3_bucket" {
  source = "terraform-aws-modules/s3-bucket/aws"

  create_bucket = false
  # ... omitted
}
```

## Examples:

* [Complete](https://github.com/terraform-aws-modules/terraform-aws-s3-bucket/tree/master/examples/complete) - Complete S3 bucket with most of supported features enabled
* [Cross-Region Replication](https://github.com/terraform-aws-modules/terraform-aws-s3-bucket/tree/master/examples/s3-replication) - S3 bucket with Cross-Region Replication (CRR) enabled

##Usage
To run this example you need to execute:

$ terraform init
$ terraform plan
$ terraform apply
Note that this example may create resources which cost money. Run terraform destroy when you don't need these resources.
