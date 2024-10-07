`main.tf`

```
module "iam_iam-user" {
  source  = "terraform-aws-modules/iam/aws//modules/iam-user"
  version = "5.28.0"
  # insert the 1 required variable here
name = "max"
}
```

`variable.tf`

```
variable "region" {
    default = "us-east-1"
}
```

`provider.tf`

```
terraform {
  required_providers {
    aws = {
      source = "hashicorp/aws"
      version = "5.11.0"
    }
  }
}

provider "aws" {
  region                      = var.region
  skip_credentials_validation = true
  skip_requesting_account_id  = true
  endpoints {
    iam = "http://aws:4566"
    ec2 = "http://aws:4566"
    s3 = "http://aws:4566"
  }
}
```
