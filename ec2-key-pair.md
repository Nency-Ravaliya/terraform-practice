`main.tf`

```
resource "aws_instance" "cerberus" {
  ami           = "ami-06178cf087598769c"
  instance_type = "m5.large"
  key_name = "cerberus"
  user_data = file("./install-nginx.sh")
}

resource "aws_key_pair" "cerberus-key" {
  key_name   = "cerberus"
  public_key = file(".ssh/cerberus.pub")
}

resource "aws_eip" "eip" {
vpc = true
instance = aws_instance.cerberus.id
provisioner "local-exec" {
command = "echo ${aws_eip.eip.public_dns} >> /root/cerberus_public_dns.txt"
}
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
