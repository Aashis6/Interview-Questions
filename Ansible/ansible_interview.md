# Question 1

What are error handlers in Ansible?

In Ansible, error handlers are tasks or blocks of tasks that are executed when a task fails during playbook execution. 
Error handlers allow you to define actions to take in response to specific errors or failures, such as retrying the task, rolling back changes,
or notifying administrators.

Error handlers are defined using the `block` structure in Ansible playbooks and can include tasks to handle various error scenarios. 
You can specify error handlers at different levels of granularity, such as for specific tasks, blocks, or entire playbooks.

Here's a basic example of using error handlers in an Ansible playbook:

```yaml
- hosts: all
  tasks:
    - name: Attempt to install a package
      yum:
        name: mypackage
        state: present
      register: result
      ignore_errors: yes
      notify: handle_error

  handlers:
    - name: handle_error
      debug:
        msg: "An error occurred during package installation"
```

In this example:

- The `yum` task attempts to install a package, and the `ignore_errors: yes` directive ensures that the playbook continues execution even
  if the task fails.
- If the `yum` task fails, the `notify: handle_error` directive triggers the `handle_error` handler.
- The `handle_error` handler is defined to print a debug message indicating that an error occurred during package installation.

Error handlers provide a mechanism for gracefully handling errors in Ansible playbooks and implementing error recovery strategies 
to ensure the robustness and reliability of your automation workflows.

# Question 2

What are different kind of handlers in ansible?

In Ansible, error handling can be achieved using various mechanisms to handle different types of errors that may occur during playbook execution. 
Some of the common error handlers in Ansible include:

1. **ignore_errors**: This handler allows Ansible to continue executing tasks even if an error occurs.
   It suppresses the error and proceeds to the next task in the playbook.

2. **failed_when**: This handler allows you to specify conditions under which a task should be considered failed.
   It lets you define custom conditions based on task outcomes.

3. **block/rescue/always**: These are task control structures that allow you to group tasks and define actions to be
    taken in case of errors. The `block` keyword defines a section of tasks to be executed, `rescue` specifies tasks to execute
   if an error occurs within the block, and `always` defines tasks to be executed regardless of whether an error occurs or not.

4. **register**: This handler allows you to capture the output or result of a task and use it for error handling or subsequent tasks.
   You can register task output into a variable and then use conditional statements to check for success or failure.

5. **failed_when**: This handler allows you to define custom conditions under which a task should be considered failed.
    It enables you to specify expressions or conditions based on task results.

6. **ignore_errors**: This handler allows you to ignore errors for specific tasks or sections of your playbook.
    It allows Ansible to continue executing tasks even if errors occur.

7. **notify**: This handler triggers another task or handler when a specific task fails or changes state.
    It allows you to define actions to be taken in response to task outcomes.

These error handling mechanisms provide flexibility in handling various types of errors and exceptions that may occur 
during playbook execution, allowing you to implement robust and resilient automation workflows in Ansible.

# Question 3

What is ansible vault and usage? 

Ansible Vault is a feature in Ansible that provides encryption for sensitive data such as passwords, API keys, and other secret information. 
It allows you to encrypt your files containing sensitive data so that they can be safely stored in version control systems or shared with others.

The main usage of Ansible Vault includes:

1. **Encrypting Sensitive Data**: You can use Ansible Vault to encrypt files containing sensitive information using the `ansible-vault encrypt`
    command.

2. **Decrypting Encrypted Files**: Ansible Vault allows you to decrypt encrypted files so that you can access and use the sensitive data within
   them using the `ansible-vault decrypt` command.

3. **Editing Encrypted Files**: You can edit encrypted files in place without having to decrypt them first using the `ansible-vault edit`
    command.

4. **Managing Encryption Passwords**: Ansible Vault enables you to manage encryption passwords securely by prompting for them interactively or
    by using encrypted files or environment variables.

5. **Integrating with Playbooks**: Encrypted files can be seamlessly integrated into Ansible playbooks using the `include_vars` directive or
    by directly referencing encrypted variables within tasks and templates.

Overall, Ansible Vault enhances the security of Ansible playbooks and configuration files by allowing you to protect sensitive data with 
encryption, ensuring that only authorized users can access and decrypt the information when needed.


Sure, here's a basic example of using Ansible Vault to encrypt a file containing sensitive data:

1. Encrypting a File:
   ```bash
   ansible-vault encrypt secrets.yml
   ```

   This command will prompt you to enter and confirm a password. After providing the password, it will encrypt the `secrets.yml` file.

2. Decrypting a File:
   ```bash
   ansible-vault decrypt secrets.yml
   ```

   This command will prompt you for the password and decrypt the `secrets.yml` file.

3. Editing an Encrypted File:
   ```bash
   ansible-vault edit secrets.yml
   ```

   This command will open the `secrets.yml` file in your default text editor, allowing you to make changes. After saving and closing the editor,
    the file will remain encrypted.

5. Viewing the Encrypted Contents:
   ```bash
   ansible-vault view secrets.yml
   ```

   This command will display the encrypted contents of the `secrets.yml` file without decrypting it.

6. Integrating with Playbooks:
   In your Ansible playbook, you can include the encrypted variables file like this:
   ```yaml
   - name: Playbook with encrypted variables
     hosts: all
     vars_files:
       - secrets.yml
     tasks:
       # Your tasks here
   ```

   Ensure that the `secrets.yml` file is encrypted before using it in the playbook.

Remember to securely manage your encryption passwords and avoid storing them in plain text. You can also use environment variables or 
encrypted password files to provide passwords to Ansible Vault.

# Question 4

What is ansible tower and its usage?

Ansible Tower is a web-based interface and automation engine that provides a dashboard to manage Ansible projects, playbooks, inventories, 
and workflows. It enhances the capabilities of Ansible by adding features such as:

1. **Graphical Interface**: Tower provides a user-friendly web-based interface for managing Ansible resources, making it easier to visualize
   and control automation tasks.

2. **Role-Based Access Control (RBAC)**: Tower allows administrators to define roles and permissions for users, ensuring secure access to
   automation resources.

3. **Job Scheduling**: It enables the scheduling of Ansible jobs to run at specific times or intervals, allowing for automated recurring tasks.

4. **Logging and Auditing**: Tower logs all Ansible job runs, providing detailed information on task execution, output, and results for
   auditing and troubleshooting purposes.

5. **Notifications**: Tower can send notifications via email, Slack, or other channels to alert users about job status changes or failures.

6. **API Access**: Tower exposes a RESTful API that allows integration with other systems and tools, enabling automation workflows to be
    incorporated into larger processes.

Overall, Ansible Tower simplifies the management and execution of Ansible automation at scale, making it a valuable tool for teams and 
organizations looking to streamline their IT operations.

# Question 5

How to skip a block from execution in ansible?

In Ansible, you can use conditional statements to skip a block from execution based on certain conditions. Here's how you can do it:

```yaml
- name: Example playbook with conditional block
  hosts: all
  tasks:
    - name: Task to check if a condition is met
      debug:
        msg: "Condition is met"
      when: some_condition_is_met

    - name: Task to skip if condition is met
      debug:
        msg: "This task will be skipped"
      when: not some_condition_is_met
```

In this example:

- The first task will execute if `some_condition_is_met` evaluates to true.
- The second task will only execute if `some_condition_is_met` evaluates to false. If the condition is met, the task will be skipped.

You can replace `some_condition_is_met` with any condition that evaluates to a boolean value (`true` or `false`).

# Question 6

How can we run command on few servers from inventory?

We can achieve this using grouping in inventory.

[dbservers]
10.10.10.10

[appserver]
10.10.10.11


# Question 7

How to  change the Ansible inventory file location?

To change the default location of the Ansible inventory file, you can specify the new location using the `ANSIBLE_INVENTORY` environment variable. Here's how you can do it:

### Method 1: Using Environment Variable

1. Open your terminal.

2. Set the `ANSIBLE_INVENTORY` environment variable to the desired location of your inventory file. For example:
   ```bash
   export ANSIBLE_INVENTORY=/path/to/your/inventory/file
   ```

3. Now, when you run Ansible commands, Ansible will look for the inventory file at the location specified in the `ANSIBLE_INVENTORY` environment variable.

### Method 2: Configuration File

Alternatively, you can set the inventory location in your Ansible configuration file (`ansible.cfg`):

1. Open your Ansible configuration file (`ansible.cfg`). If it doesn't exist, you can create one in your project directory or in `/etc/ansible`.

2. Add the following line to specify the inventory location:
   ```ini
   inventory = /path/to/your/inventory/file
   ```

3. Save the configuration file.

With either of these methods, Ansible will use the specified location for the inventory file instead of the default location (`/etc/ansible/hosts` or `./hosts` in the current directory).
