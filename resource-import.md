`main.tf`

```
resource "aws_instance" "ruby" {
  ami           = var.ami
  instance_type = var.instance_type
  for_each      = var.name
  key_name      = var.key_name
  tags = {
    Name = each.value
  }
}

resource "aws_instance" "jade-mw" {
#jsk
ami = "ami-082b3eca746b12a89"
instance_type = "t2.large"
key_name = "jade"
tags = {Name = "jade-mw"}
}

output "instances" {
  value = aws_instance.ruby
}
```

`variable.tf`

```
variable "name" {
  type    = set(string)
  default = ["jade-webserver", "jade-lbr", "jade-app1", "jade-agent", "jade-app2"]

}
variable "ami" {
  default = "ami-0c9bfc21ac5bf10eb"
}
variable "instance_type" {
  default = "t2.nano"
}
variable "key_name" {
  default = "jade"

}
```

`provider.tf`

```
terraform {
  required_providers {
    aws = {
      source = "hashicorp/aws"
      version = "4.15.0"
    }
  }
}

provider "aws" {
  region                      = "us-east-1"
  skip_credentials_validation = true
  skip_requesting_account_id  = true

  endpoints {
    ec2 = "http://aws:4566"
  }
}
```
