In Terraform, **lifecycle rules** are used to control how resources are created, updated, or deleted. They help manage the behavior of resources when changes are made in your infrastructure.

### Key Lifecycle Rules:

1. **`create_before_destroy`**: 
   - Creates a new resource **before** destroying the old one. This ensures no downtime, as the new resource is ready before removing the old one.

2. **`prevent_destroy`**:
   - Prevents Terraform from **accidentally deleting** a resource, adding an extra layer of protection to avoid unintentional destruction.

3. **`ignore_changes`**:
   - Tells Terraform to **ignore certain attributes** of a resource, even if they change, preventing unnecessary updates.

### Simple Example:
```hcl
resource "aws_instance" "example" {
  ami           = "ami-123456"
  instance_type = "t2.micro"

  lifecycle {
    create_before_destroy = true
    prevent_destroy        = true
  }
}
```

- In this example, Terraform will create a new instance before destroying the old one and will prevent accidental deletion of the instance.
