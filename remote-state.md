`main.tf`

```
resource "local_file" "state" {
filename="/root/${var.local-state}"
content="This configuration uses ${var.local-state} state"
}
```

`variable.tf`

```
variable remote-state {
    type = string
    default = "remote"
}
variable local-state {
    type = string
    default = "local"
}
```
---

## Remote backend conf on MiniO Console

main.tf + variable.tf

`terraform.tf`

```
terraform {
  backend "s3" {
    key = "terraform.tfstate"
    region = "us-east-1"
    bucket = "remote-state"
    endpoint = "http://172.16.238.105:9000"
    force_path_style = true


    skip_credentials_validation = true

    skip_metadata_api_check = true
    skip_region_validation = true
  }
}
```
