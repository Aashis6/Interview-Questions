# Question 1: 

What is the usage of Depends on flag in terraform? 

In Terraform, the depends_on meta-argument is used to specify explicit dependencies between resources. 
It ensures that certain resources are created or destroyed in a specific order relative to other resources.

resource "aws_instance" "example" {
  #Define your instance configuration here
}

resource "aws_eip" "example" {
  #Define your Elastic IP configuration here

  #This EIP resource depends on the creation of the instance
  depends_on = [aws_instance.example]
}

In this example, the Elastic IP resource aws_eip.example depends on the creation of the EC2 instance aws_instance.example. 
This means that Terraform will ensure the instance is created before attempting to create the Elastic IP.

# Question 2:

What is the terraform workspaces and how to switch between workspaces?

Terraform workspaces are a feature that allows you to manage multiple distinct sets of infrastructure resources within
a single configuration. Each workspace maintains its own state, allowing you to manage environments such as development,
staging, and production separately.

To switch between workspaces in Terraform, you can use the `terraform workspace` command. Here's how you can use it:

1. **List Workspaces**: To list all available workspaces, you can use the following command:
   ```
   terraform workspace list
   ```

2. **Create a New Workspace**: If you need to create a new workspace, you can use the following command:
   ```
   terraform workspace new <workspace_name>
   ```

3. **Select a Workspace**: To switch to an existing workspace, use the following command:
   ```
   terraform workspace select <workspace_name>
   ```

4. **Show Current Workspace**: To display the current workspace, you can use the following command:
   ```
   terraform workspace show
   ```

5. **Delete a Workspace**: To delete a workspace, use the following command:
   ```
   terraform workspace delete <workspace_name>
   ```

Remember to use these commands carefully, as they can have implications on your infrastructure state and configurations. 
Always ensure that you are working in the correct workspace before making changes.

# Question 3

What is dynamic block in terraform?

Dynamic blocks in Terraform allow you to define repeating nested blocks dynamically, based on the elements of a list, 
map, or set within your configuration. 
This feature provides flexibility when you need to generate multiple similar blocks with different configurations.

It is used for eg. Ingress blocks where multiple ports are to be allowed. We can use dynamic block to achieve this.

# Question 4

What is the difference between for and for each in terraform?

In Terraform, both `for` and `for_each` are used for looping over elements, but they serve different purposes:

1. **`for`**: The `for` expression is used for generating a sequence of values or transforming one collection into another.
2. It's typically used for generating a list, map, or set of values based on some criteria.

    Example:
    ```hcl
    locals {
      numbers = [1, 2, 3, 4, 5]
      doubled = [for n in local.numbers : n * 2]
    }
    ```

    In this example, `for n in local.numbers : n * 2` generates a new list where each element of `local.numbers` is doubled.

3. **`for_each`**: The `for_each` expression is used in resource or module blocks to
4.  create multiple instances of a resource or module based on a map, set, or list of objects.

    Example:
    ```hcl
    resource "aws_instance" "servers" {
      for_each = {
        web-1 = { ami = "ami-123456", instance_type = "t2.micro" }
        web-2 = { ami = "ami-789012", instance_type = "t2.micro" }
      }

      ami           = each.value.ami
      instance_type = each.value.instance_type
    }
    ```

    In this example, `for_each` creates two instances of the `aws_instance` resource, one for each key-value pair in the map.
    The keys (`web-1`, `web-2`) are used as unique identifiers for each instance, and the values provide the configuration for each instance.

In summary, `for` is used for list comprehension and transformation,
while `for_each` is used for resource or module instantiation with unique identifiers.


# Question 5

Where do we store the tfstate file? and how to enable locking mechanism.

We can store the tfstate file in s3 using the backend. Locking mechanism can be enabled using the dynamodb_table.

```terraform {
  backend "s3" {
    bucket         = "my-terraform-state-bucket"
    key            = "terraform.tfstate"
    region         = "us-east-1"
    encrypt        = true
    dynamodb_table = "terraform-lock"
  }
}
```

# Question 5

Command to destroy only a particular resource.

To destroy only one particular resource using Terraform, you can use the `-target` flag with the `terraform destroy` command. 
This flag allows you to specify the resource you want to destroy without affecting other resources managed by your Terraform configuration.

Here's the syntax:

```bash
terraform destroy -target=resource_type.resource_name
```

Replace `resource_type` with the type of the resource you want to destroy (e.g., `aws_instance`, `aws_s3_bucket`, etc.), 
and `resource_name` with the name of the resource as defined in your Terraform configuration.

For example, if you want to destroy an AWS EC2 instance named `my_instance`, you would run:

```bash
terraform destroy -target=aws_instance.my_instance
```

This command will only destroy the specified resource, leaving other resources managed by your Terraform configuration intact.
However, be cautious when using the `-target` flag, as it can lead to unintended consequences if used improperly.

# Question 6

Command to create only a particular resource.

To apply changes only to a specific resource using Terraform, you can use the `-target` option with the `terraform apply` command. 
This option allows you to specify a single
resource or module to apply changes to, rather than applying changes to all resources in your Terraform configuration.

Here's the syntax:

```bash
terraform apply -target=resource_type.resource_name
```

Replace `resource_type.resource_name` with the resource type and name of the specific resource you want to apply changes to.

For example, if you have an AWS EC2 instance defined in your Terraform configuration as:

```hcl
resource "aws_instance" "example" {
  # Configuration for your EC2 instance
}
```

You can apply changes only to this instance by running:

```bash
terraform apply -target=aws_instance.example
```

This command will apply changes only to the specified EC2 instance, leaving other resources unchanged.

However, please note that using `-target` can have unintended consequences, especially in
complex environments with interdependencies between resources. It's generally recommended to use it with caution and understand its implications on your infrastructure.

# Question 7

What are terraform provisioners?

Terraform provisioners are a feature that allow you to run additional commands or scripts on local or remote resources after they have been created by Terraform. They are typically used for tasks such as installing software, configuring resources, or running tests after resource creation.

There are several types of provisioners in Terraform:

1. **local-exec**: This provisioner executes commands locally on the machine running Terraform. It is useful for tasks that need to be performed on the machine where Terraform is being executed, such as running shell scripts or executing other local commands.

2. **remote-exec**: This provisioner connects to remote resources via SSH or WinRM and executes commands on them. It is commonly used for tasks like configuring newly created virtual machines or instances by running commands on them remotely.

3. **file**: The file provisioner is used to copy files or directories from the machine running Terraform to remote resources created by Terraform. This can be useful for transferring configuration files, scripts, or other resources to provisioned instances.

Provisioners are defined within a `provisioner` block inside a resource definition in a Terraform configuration file. Here's an example of how provisioners can be used:

```hcl
resource "aws_instance" "example" {
  ami           = "ami-12345678"
  instance_type = "t2.micro"

  provisioner "local-exec" {
    command = "echo Hello, World > hello.txt"
  }

  provisioner "remote-exec" {
    inline = [
      "sudo apt-get update",
      "sudo apt-get install -y nginx",
      "sudo systemctl start nginx"
    ]
  }

  provisioner "file" {
    source      = "local/path/to/file"
    destination = "/remote/path/to/file"
  }
}
```

In this example:

- The `local-exec` provisioner writes "Hello, World" to a file named `hello.txt` on the local machine.
- The `remote-exec` provisioner connects to the created AWS instance via SSH and installs Nginx.
- The `file` provisioner copies a file from the local machine to the remote instance.

It's important to use provisioners judiciously, as they can introduce dependencies and make Terraform configurations less idempotent. In general, it's recommended to use configuration management tools like Ansible or Chef for more complex provisioning tasks, and reserve Terraform provisioners for lightweight tasks or temporary workarounds.

# Question 8

How to store secrets in Terraform?

In Terraform, secrets can be stored using various methods, each with its own advantages and considerations:

1. **Sensitive Input Variables**: Terraform supports sensitive input variables, which are marked as sensitive in the Terraform configuration. These variables are not shown in the Terraform output, state file, or logs. They can be passed via command-line flags, environment variables, or input files.

   Example:
   ```hcl
   variable "secret_password" {
     type        = string
     description = "Password for the application"
     sensitive   = true
   }
   ```

2. **Using Environment Variables**: Secrets can be passed to Terraform via environment variables. While this approach is simple, it may not be suitable for all use cases, especially when dealing with large numbers of secrets or sensitive information.

   Example:
   ```bash
   export TF_VAR_secret_password="supersecret"
   ```

3. **External Secret Management Systems**: Organizations can use external secret management systems like HashiCorp Vault, AWS Secrets Manager, or Azure Key Vault to store and manage secrets. Terraform can integrate with these systems to fetch secrets during execution.

   Example (HashiCorp Vault):
   ```hcl
   data "vault_generic_secret" "example" {
     path = "secret/data/myapp"
   }
   ```

4. **Encrypted State Files**: Terraform state files can be encrypted using tools like Terraform Cloud, Terraform Enterprise, or third-party encryption solutions. This adds an extra layer of security to sensitive data stored in the state.

   Example (Terraform Cloud):
   ```hcl
   terraform {
     backend "remote" {
       organization = "myorg"
       workspaces {
         name = "example"
       }
     }
   }
   ```

When storing secrets in Terraform, it's essential to follow security best practices, such as limiting access to sensitive data, rotating credentials regularly, and monitoring access to secrets. Additionally, consider the compliance requirements and regulations governing the handling of sensitive information in your organization.

# Question 9

How will you upgrade plugins on Terraform?

Terraform init --upgrade

# Question 10

How will you control and handle rollbacks when something goes wrong?

Maintain VCS for the previous working code. Pull it to machine and execute the ```terraform apply``` again

# Question 11

What does TF_LOG do?

The `TF_LOG` environment variable in Terraform controls the verbosity of Terraform's logging output. It allows you to specify the logging level for Terraform operations, which can be helpful for troubleshooting and debugging Terraform configurations and executions.

The `TF_LOG` variable accepts four different log levels:

1. `TRACE`: Provides the most detailed logging, including detailed information about each operation Terraform performs. This level is useful for deep debugging but can generate a large amount of output.
2. `DEBUG`: Provides debug-level logging, including information about Terraform's internal operations and decisions. It's helpful for troubleshooting complex issues.
3. `INFO`: Provides informational logging, including summaries of Terraform operations and status updates. It's the default log level and provides a good balance between verbosity and readability.
4. `WARN`: Provides warnings about potential issues or configuration problems. It's useful for highlighting warnings that might impact the execution of Terraform operations.
5. `ERROR`: Provides only error messages, indicating critical issues that prevent Terraform from completing operations successfully. This level is useful for identifying and resolving errors quickly.

You can set the `TF_LOG` environment variable to one of these levels to control the amount of logging output produced by Terraform. For example:

```bash
export TF_LOG=DEBUG
```

This command sets the logging level to `DEBUG`, causing Terraform to output debug-level messages during execution. Alternatively, you can set it inline with the Terraform command:

```bash
TF_LOG=DEBUG terraform apply
```

This command runs the `terraform apply` command with debug-level logging enabled for that specific execution.

# Questuon 12
Which command can be used to reconcile the Terraform state with the actual real-world infrastructure?

```terraform refresh```

# Question 13

What is tainted resource in terraform?

In Terraform, a "tainted" resource refers to a resource that Terraform knows to be in an inconsistent or unexpected state due to external factors. This could be because the resource was manually modified outside of Terraform, or because of some other reason that caused the resource to deviate from its expected configuration.

When you mark a resource as tainted using the `terraform taint` command, Terraform will consider that resource as needing to be recreated during the next `terraform apply` operation. Terraform will destroy and recreate the tainted resource to bring it back into the desired state as defined in your configuration.

Use cases for tainting a resource include:

1. Correcting manual changes: If someone manually modifies a resource provisioned by Terraform (e.g., making changes directly in the AWS Management Console), you can mark the resource as tainted to ensure that Terraform brings it back into the desired state defined in your configuration.

2. Handling drift detection: Tainting resources can be part of a strategy to detect and correct configuration drift between your infrastructure as defined in your Terraform configuration and the actual state of your resources in the cloud provider. This helps maintain consistency and reproducibility in your infrastructure.

3. Reapplying specific resources: In some cases, you may want to apply changes only to certain resources without affecting others. Tainting specific resources allows you to target those resources for recreation during the next apply operation.

Overall, tainting resources in Terraform provides a way to manage and correct deviations from the desired state of your infrastructure configuration.

# Question 14

Difference between variable and local variable in terraform?

In Terraform, both variables and locals are ways to define and use values within your configuration, but they serve different purposes:

1. **Variables:**
   - Variables are used to parameterize your Terraform configuration, allowing you to customize the behavior of your infrastructure.
   - They are defined using the `variable` block in Terraform configuration files.
   - Variables can be defined at various levels of granularity: globally, within modules, or within resources.
   - They can have default values, descriptions, and type constraints.
   - Variables are typically provided values by the user via input variables, which can be specified in various ways, such as through command-line flags, environment variables, or variable files.
   - Example:
     ```hcl
     variable "instance_type" {
       description = "The EC2 instance type"
       type        = string
       default     = "t2.micro"
     }

     resource "aws_instance" "example" {
       instance_type = var.instance_type
       # other configuration settings...
     }
     ```

2. **Locals:**
   - Locals are used to define values that are derived from other values within your configuration, allowing you to avoid repeating complex expressions.
   - They are defined using the `locals` block in Terraform configuration files.
   - Locals are evaluated once per Terraform run and are not exposed outside of the configuration where they are defined.
   - They can be used to store intermediate results, construct complex values, or avoid repetition of expressions.
   - Example:
     ```hcl
     locals {
       subnet_ids = {
         "us-west-1a" = "subnet-12345678"
         "us-west-1b" = "subnet-87654321"
       }
     }

     resource "aws_instance" "example" {
       subnet_id = local.subnet_ids[var.availability_zone]
       # other configuration settings...
     }
     ```

In summary, variables are used for parameterization and customization of your infrastructure, while locals are used for intermediate computations and to avoid repetition of complex expressions. Both are important tools for managing and organizing your Terraform configurations effectively.

# Question 15

What is null response in terraform?

In Terraform, a null response refers to a special value that represents the absence of a valid value. It is commonly used in Terraform configurations to handle situations where a resource or data object may not exist or may not have a value.

A null response can occur in various scenarios:

1. **Data Not Found**: When querying for data using Terraform data sources, the data source may return null if the requested data does not exist.

2. **Conditional Values**: In Terraform configurations, you may use conditional expressions to evaluate certain conditions and return null if the condition is not met.

3. **Resource Outputs**: If a Terraform resource does not have any outputs or if the outputs are not explicitly defined, accessing those outputs may result in a null value.

4. **Terraform Functions**: Certain Terraform functions may return null in specific scenarios. For example, the `lookup` function returns null if the specified key does not exist in the map.

Handling null responses in Terraform typically involves using conditional expressions or built-in functions to check for null values and handle them appropriately. For example, you can use the `coalesce` function to return the first non-null value from a list of expressions, or you can use the `if` function to conditionally evaluate expressions based on whether a value is null or not.

Here's an example of using the `if` function to handle a null response:

```hcl
variable "my_data" {
  type = map
  default = {
    key1 = "value1"
    key2 = "value2"
  }
}

output "example_output" {
  value = if contains(var.my_data, "key3") {
    var.my_data["key3"]
  } else {
    null
  }
}
```

In this example, the `example_output` will be assigned the value of `var.my_data["key3"]` if the key exists in the map `var.my_data`, otherwise it will be assigned null. This helps to handle situations where the requested key may not exist in the map.
