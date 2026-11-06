# Linux User and Permission Management

## Objective
Practice Linux user administration, group management, directory permissions, access restrictions, and group inheritance.

## Environment
- OS: Ubuntu 24.04.5 LTS
- Environment: WSL2
- Shell: Bash

## 1. User and Group Management
Created a dedicated user `laboperator` and group `labteam`.

Commands used:
- `groupadd`
- `useradd`
- `usermod`
- `id`

## 2. Directory Permissions
Created `/opt/access-control-lab`.

- Owner: root
- Group: labteam
- Permissions: 770

## 3. Access Control Testing
- `laboperator` successfully created files in the shared directory.
- An unauthorized user (`nobody`) was denied access.
- An initial attempt under `/home/jahid` failed because the user lacked permission to traverse a parent directory. Moving the shared directory to `/opt` resolved this.

## 4. Group Inheritance
Enabled the setgid bit using `chmod g+s`.

New files created inside the directory inherited the `labteam` group.

## 5. Skills Demonstrated
- Linux user and group administration
- File ownership with `chown`
- Permissions with `chmod`
- Access-control troubleshooting
- setgid directory configuration
- Permission verification

## 6. Conclusion
Configured and tested a restricted shared directory, verified authorized and unauthorized access, and demonstrated automatic group inheritance.