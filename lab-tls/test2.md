Within this directory, create two local_file type resources in main.tf file.


Resource 1:

Resource Name: whale

File Name: /root/whale

content: whale


Resource 2:

Resource Name: krill

File Name: /root/krill

content: krill


Resource called whale should depend on krill but do not use reference expressions.


When ready, run terraform init, plan and apply.

## solution

`main.tf`

```
resource "local_file" "whale" {
    filename = "/root/whale"
    content = "whale"

    depends_on = [
        local_file.krill
    ]
}

resource "local_file" "krill" {
    filename = "/root/krill"
    content = "krill"
}

```
