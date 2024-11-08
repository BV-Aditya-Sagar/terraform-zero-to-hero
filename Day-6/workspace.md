Terraform workspaces allow you to manage multiple environments (like **development**, **staging**, and **production**) within the same Terraform configuration. Each workspace provides a separate state file, meaning that the infrastructure you create in one workspace is independent of the others, even though they use the same codebase.

### Key Features of Terraform Workspaces
1. **Separate State Files**: Each workspace has its own state file, which keeps the resources in each environment separate.
2. **Isolation for Different Environments**: Workspaces are commonly used to handle different stages of deployment (e.g., dev, staging, prod) without creating entirely separate configurations.
3. **Environment-Specific Variables**: Workspaces make it easy to manage environment-specific variables, such as instance types or database configurations, by referencing `terraform.workspace` within the configuration.

### Default and Custom Workspaces
- **Default Workspace**: When you initialize a new Terraform configuration, Terraform creates a `default` workspace automatically.
- **Custom Workspaces**: You can create custom workspaces, such as `dev`, `staging`, and `prod`, to handle multiple environments.

### Basic Commands for Workspaces

1. **Create a New Workspace**:
   ```bash
   terraform workspace new dev
   ```

2. **Switch to an Existing Workspace**:
   ```bash
   terraform workspace select dev
   ```

3. **List All Workspaces**:
   ```bash
   terraform workspace list
   ```

4. **Show the Current Workspace**:
   ```bash
   terraform workspace show
   ```

### Example Usage in Terraform Configuration

Let's say you want to set different instance types based on the workspace environment. You can define a variable map and use `terraform.workspace` to reference it.

```hcl
variable "instance_type" {
  type    = map(string)
  default = {
    dev     = "t2.micro"
    staging = "t2.small"
    prod    = "t2.medium"
  }
}

resource "aws_instance" "example" {
  ami           = "ami-09b0a86a2c84101e1"
  instance_type = var.instance_type[terraform.workspace]
}
```

In this example:
- When you switch to the `dev` workspace, Terraform will use `t2.micro` as the instance type.
- When you switch to the `prod` workspace, it will use `t2.medium`.

### When to Use Workspaces
Workspaces are useful for:
- Managing separate environments (e.g., dev, staging, prod) in one configuration.
- Running isolated test environments in parallel.

However, for highly complex environments with different configurations, it may be more manageable to have separate directories or repositories rather than using workspaces alone. 

### Limitations of Workspaces
Workspaces do have some limitations:
- They are mainly a state management solution and do not completely isolate all aspects of infrastructure.
- Workspaces are less effective when environments have drastically different configurations, as they rely on the same codebase.
