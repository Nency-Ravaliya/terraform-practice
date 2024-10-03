`main.tf`

```
resource "local_file" "file" {
    filename = var.filename
    file_permission =  var.permission
    content = random_string.string.id
    
}

resource "random_string" "string" {
    length = var.length
    keepers = {
        length = var.length
    }  
    
}
```

`variable.tf`

```
variable "length" {
    default = 12
  
}
variable "filename" {
    default = "/root/random_text"
}
variable "content" {
    default = "This file contains a single line of data"
}
variable "permission" {
    default = 0770
}
```
