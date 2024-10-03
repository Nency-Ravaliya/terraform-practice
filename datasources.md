In Terraform, **data sources** are a way to reference and retrieve information from existing resources that are not managed by your Terraform configuration. They allow you to access external data or resources, which can then be used to configure your Terraform-managed infrastructure.

### Key Points about Data Sources:

1. **Read-Only**: Data sources are **read-only**; you cannot modify the data they reference. They are used to fetch information about existing infrastructure.

2. **Use Cases**: Data sources are commonly used to:
   - Retrieve information about existing resources, like AMIs (Amazon Machine Images), VPCs, or security groups.
   - Dynamically fetch information that may change over time, such as the latest AMI ID for a specific operating system.
   - Share configuration data between modules without directly managing the resource in your Terraform code.

3. **Syntax**: Data sources are defined using the `data` block, specifying the provider and type of resource.

### Example:
Here’s a simple example of using a data source in Terraform to retrieve an existing AWS AMI ID:

```hcl
provider "aws" {
  region = "us-east-1"
}

# Data source to get the latest Amazon Linux 2 AMI
data "aws_ami" "latest_amazon_linux" {
  most_recent = true
  owners      = ["amazon"]

  filter {
    name   = "name"
    values = ["amzn2-ami-hvm-*-x86_64-gp2"]
  }
}

# Use the retrieved AMI ID in an EC2 instance
resource "aws_instance" "example" {
  ami           = data.aws_ami.latest_amazon_linux.id
  instance_type = "t2.micro"
}
```

### Summary:
- **Data Sources** allow you to fetch information about existing resources that Terraform does not manage.
- They are useful for dynamically referencing resources and sharing configurations.
- Defined using the `data` block, they enable you to access properties of resources across your infrastructure setup.

### ex1

`main.tf`

```
output "os-version" {
  value = data.local_file.os.content
}

data "local_file" "os" {
  filename = "/etc/os-release"
}
```

### ex2

`ebs.tf`

```
data "aws_ebs_volume" "gp2_volume" {
  most_recent = true

  filter {
    name   = "volume-type"
    values = ["gp2"]
  }
}
```

### ex3

`s3.tf`

```
data "aws_s3_bucket" "selected" {
  bucket_name = "bucket.test.com"
}
```
