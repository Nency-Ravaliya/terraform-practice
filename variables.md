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
---

In Terraform, there are several ways to define and pass values to variables, making it flexible and easy to manage configurations. Here are the **five main ways** to define and provide values for variables in Terraform:

### 1. **Using Default Values in the `variable` Block**
   - You can define variables with a default value directly in the configuration. If no value is provided by the user, Terraform will use this default.
   - **Example**:
     ```hcl
     variable "instance_type" {
       type    = string
       default = "t2.micro"
     }
     ```

### 2. **Command Line Arguments (`-var`)**
   - You can pass values to variables directly from the command line using the `-var` flag during `terraform plan` or `terraform apply`.
   - **Example**:
     ```bash
     terraform apply -var="instance_type=t2.small" -var="instance_count=2"
     ```
   - **Explanation**: This allows you to override the default values by providing variable values dynamically when running Terraform commands.

### 3. **Variable Definition Files (`.tfvars` or `.tfvars.json`)**
   - Terraform allows you to define variable values in a separate file with either a `.tfvars` or `.tfvars.json` extension. You can then reference these files during the execution of Terraform.
   - **Example (`terraform.tfvars`)**:
     ```hcl
     instance_type = "t2.small"
     instance_count = 2
     ```
   - **Example (`terraform.tfvars.json`)**:
     ```json
     {
       "instance_type": "t2.small",
       "instance_count": 2
     }
     ```
   - **Usage**:
     ```bash
     terraform apply -var-file="terraform.tfvars"
     ```

   - **Automatic Detection**: Terraform will automatically load a file named `terraform.tfvars` or `*.auto.tfvars` if they are in the root directory.

### 4. **Environment Variables (`TF_VAR_name`)**
   - You can set variables via environment variables using the prefix `TF_VAR_`. This allows you to pass variable values without explicitly defining them in your configuration or passing them via the command line.
   - **Example**:
     ```bash
     export TF_VAR_instance_type="t2.large"
     export TF_VAR_instance_count="3"
     terraform apply
     ```

   - **Explanation**: Environment variables are a convenient way to manage sensitive values like API keys or secrets without embedding them in your Terraform configuration.

### 5. **Terraform Cloud or Enterprise Workspaces**
   - If you're using Terraform Cloud or Enterprise, you can define variables via the web UI or API. This allows you to manage variables in a centralized place and use them across multiple environments.
   - **How it works**: Terraform Cloud Workspaces provide a user interface where you can input variables, or you can specify them via environment variables within the cloud platform itself.

---

### Summary of Ways to Define Variables:

| Method                        | Description                                                                                                                                  | Example Usage                                                   |
|-------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------|
| **Default Values**             | Defined in the `variable` block in the `.tf` file. If no value is provided, the default value is used.                                       | Define directly in `.tf` file with `default = "value"`.         |
| **Command Line (`-var`)**      | Pass variable values directly on the command line using the `-var` flag.                                                                     | `terraform apply -var="instance_type=t2.small"`                 |
| **Variable Definition Files**  | Define variables in a `.tfvars` or `.tfvars.json` file. Terraform automatically loads `terraform.tfvars` or `*.auto.tfvars` if present.      | Create a `terraform.tfvars` or `prod.auto.tfvars` file.         |
| **Environment Variables**      | Define variables as environment variables using the `TF_VAR_` prefix.                                                                        | `export TF_VAR_instance_type="t2.small"`; `terraform apply`     |
| **Terraform Cloud Workspaces** | Use Terraform Cloud/Enterprise to define variables centrally via the UI or API, used across multiple environments or teams.                  | Set variables via the Terraform Cloud web UI or API.            |

---

### Example of Combining Methods

You can combine different methods to override values at different levels. Terraform applies values in this order of precedence:

1. Command-line arguments (`-var` or `-var-file`).
2. Environment variables (`TF_VAR_`).
3. `.tfvars` files (like `terraform.tfvars` or `*.auto.tfvars`).
4. Default values in the `variable` block.

**Example Workflow**:
```hcl
# Define variable with default value
variable "instance_type" {
  type    = string
  default = "t2.micro"
}

# You have a terraform.tfvars file with the following content:
# instance_type = "t2.small"

# You set an environment variable to override it:
# export TF_VAR_instance_type="t2.medium"

# Finally, you can override everything with a command-line flag:
# terraform apply -var="instance_type=t2.large"
```

In this example, `t2.large` will be used because command-line arguments have the highest precedence.
