# Troubleshoot SELinux Policies
#### Install policycoreutils
This is required for SE policy commands
```bash
dnf install policycoreutils-python-utils
```
#### Audit commands
```bash
# Provides the error message for why SELinux is blocking the app/service.
audit2why -a

# Provides the allow policy for fixing the issue.
audit2allow -a
```

#### Create and Install custom policy
The following commands looks through the SELinux logs for the respective service and creates a policy, that grants permission to the app.
```bash
ausearch -c '<service>' --raw | audit2allow -M my-<service>
semodule -i my-<service>.pp
```