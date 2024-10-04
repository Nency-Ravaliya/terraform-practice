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
