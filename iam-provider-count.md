`iam-user.tf`

```
resource "aws_iam_user" "users" {
  name  = var.project-sapphire-users[count.index]
  count = length(var.project-sapphire-users)
  tags = {
    Description = "Team Lead"

  }

}
```

`variable.tf`

```
variable "project-sapphire-users" {
  type    = list(string)
  default = ["mary", "jack", "jill", "mack", "buzz", "mater"]
}
```

`provider.tf`

```
provider "aws" {
  region                      = "us-east-1"
  skip_credentials_validation = true
  skip_requesting_account_id  = true

  endpoints {
    iam = "http://aws:4566"
  }
}
```
