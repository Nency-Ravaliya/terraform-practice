In Terraform, **variables** allow you to parameterize your configurations, making them reusable and more dynamic. You can define variables to pass values dynamically, which helps in customizing configurations without hardcoding values.

### Types of Variables in Terraform
Terraform supports various types of variables to manage input values. The main types are:

1. **String**
2. **Number**
3. **Bool**
4. **List**
5. **Map**
6. **Set**
7. **Object**
8. **Tuple**

---

### 1. **String**
   - **Description**: A single-line text value.
   - **Example**:
     ```hcl
     variable "instance_type" {
       type    = string
       default = "t2.micro"
     }
     ```
   - **Usage**:
     ```hcl
     resource "aws_instance" "example" {
       instance_type = var.instance_type
     }
     ```

### 2. **Number**
   - **Description**: Numeric values (integers or floating-point numbers).
   - **Example**:
     ```hcl
     variable "instance_count" {
       type    = number
       default = 2
     }
     ```
   - **Usage**:
     ```hcl
     resource "aws_instance" "example" {
       count = var.instance_count
     }
     ```

### 3. **Bool**
   - **Description**: Boolean values (`true` or `false`).
   - **Example**:
     ```hcl
     variable "enable_monitoring" {
       type    = bool
       default = true
     }
     ```
   - **Usage**:
     ```hcl
     resource "aws_instance" "example" {
       monitoring = var.enable_monitoring
     }
     ```

### 4. **List**
   - **Description**: A collection of values, ordered, and of the same type.
   - **Example**:
     ```hcl
     variable "availability_zones" {
       type    = list(string)
       default = ["us-west-1a", "us-west-1b"]
     }
     ```
   - **Usage**:
     ```hcl
     resource "aws_instance" "example" {
       availability_zone = element(var.availability_zones, 0)
     }
     ```

### 5. **Map**
   - **Description**: A collection of key-value pairs, where keys are unique, and all values are of the same type.
   - **Example**:
     ```hcl
     variable "instance_ami" {
       type = map(string)
       default = {
         us-east-1 = "ami-123456"
         us-west-1 = "ami-654321"
       }
     }
     ```
   - **Usage**:
     ```hcl
     resource "aws_instance" "example" {
       ami = var.instance_ami["us-east-1"]
     }
     ```

### 6. **Set**
   - **Description**: A collection of unique values of the same type, unordered.
   - **Example**:
     ```hcl
     variable "instance_types" {
       type = set(string)
       default = ["t2.micro", "t2.small"]
     }
     ```
   - **Usage**:
     ```hcl
     resource "aws_instance" "example" {
       instance_type = element(var.instance_types, 0)
     }
     ```

### 7. **Object**
   - **Description**: A complex structure that contains attributes, which can be of different types.
   - **Example**:
     ```hcl
     variable "server" {
       type = object({
         name    = string
         cpu     = number
         enabled = bool
       })
       default = {
         name    = "web"
         cpu     = 2
         enabled = true
       }
     }
     ```
   - **Usage**:
     ```hcl
     resource "aws_instance" "example" {
       instance_type = "t2.micro"
       tags = {
         Name = var.server.name
       }
     }
     ```

### 8. **Tuple**
   - **Description**: A sequence of values, ordered, and possibly of different types.
   - **Example**:
     ```hcl
     variable "server_details" {
       type    = tuple([string, number, bool])
       default = ["web-server", 2, true]
     }
     ```
   - **Usage**:
     ```hcl
     output "server_name" {
       value = element(var.server_details, 0)
     }
     ```

---

### Example of Variable Definition and Usage

```hcl
# Define variables
variable "instance_type" {
  type    = string
  default = "t2.micro"
}

variable "instance_count" {
  type    = number
  default = 2
}

variable "availability_zones" {
  type    = list(string)
  default = ["us-east-1a", "us-east-1b"]
}

# Use variables in a resource
resource "aws_instance" "example" {
  count         = var.instance_count
  instance_type = var.instance_type
  availability_zone = element(var.availability_zones, count.index)
}
```

### Ways to Assign Values to Variables
1. **Default Value**: Defined in the variable block using `default`.
2. **Command Line Input**:
   ```bash
   terraform apply -var="instance_type=t2.small"
   ```
3. **Variable Files**:
   You can define variables in a separate `.tfvars` file:
   ```hcl
   # terraform.tfvars
   instance_type = "t2.small"
   instance_count = 3
   ```
4. **Environment Variables**:
   You can pass variables using environment variables:
   ```bash
   export TF_VAR_instance_type="t2.large"
   terraform apply
   ```

### Summary

- **String**: Single text values.
- **Number**: Numeric values.
- **Bool**: Boolean values (`true` or `false`).
- **List**: Ordered collection of the same type.
- **Map**: Key-value pairs of the same type.
- **Set**: Unordered, unique collection of values.
- **Object**: Custom structure with multiple attributes of different types.
- **Tuple**: Ordered collection with possibly different types.

Variables in Terraform allow you to make your configurations dynamic, reusable, and flexible by passing values during runtime.
