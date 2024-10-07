`main.tf`

```
resource "aws_instance" "cerberus" {
  ami           = "ami-06178cf087598769c"
  instance_type = "m5.large"
  key_name = "cerberus"
}

resource "aws_key_pair" "cerberus-key" {
  key_name   = "cerberus"
  public_key = file(".ssh/cerberus.pub")
}
```

`variable.tf`

```
variable "region" {
  default = "eu-west-2"
}
```

`install-nginx.sh`

```
#!/bin/bash
sudo yum update -y
sudo yum install nginx -y
sudo systemctl start nginx
```
