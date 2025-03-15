# Debian hardening: 
Here are few steps you can follow if you want to harden your debian system. This is not an extensive checklist. This is just a centralized and reachable resource for me. 

## Resources:
The main resource I suggest if you have some time is the [Securing Debian Manual](https://www.debian.org/doc/manuals/securing-debian-manual/index.en.html) from the project. 
I mainly used these works that presents more synthetic measures:  
- [Sécurisation d'un serveur Linux sous Debian](https://hackmd.io/@Ben-Rahiti-Romain/SkciYWMWj) that did the same work with the ANSSI paper in French and gave implementations methods for level 1 & 2.
- [Configuration recommendations od a gnu/linux system](https://cyber.gouv.fr/sites/default/files/document/linux_configuration-en-v2.pdf)
- [Openssh secure use recommendations](https://cyber.gouv.fr/en/publications/openssh-secure-use-recommendations)

## Checklist: 
### Level 1:
    - [ ] Removing the unused user accounts
    - [ ] User password strength
    - [ ] Avoiding files or directories without a known user or group
    - [ ] Setting the sticky bit on the writable directories
    - [ ] Avoiding using executables with setuid and setguid rights
    - [ ] Installing only strictly necessary packages
    - [ ] Using only official package repositories
    - [ ] Updating regularly the system
    - [ ] Disabling the non-necessary services
    - [ ] Protecting the stored passwords
    - [ ] Minimizing the attack surface of network services

### ssh connections:
    - [ ] Deactivate root connection for ssh
    - [ ] Deactivate Password authentication only for ssh (only use key authentication).
    - [ ] Add a 2FA layer with hardware security keys (yubicos, ledger, etc...) for ssh. Or use both key + password authentication.
    - [ ] If your setup allows this, allow the admin ssh connection only from a specific IP.
    - [ ] Lock and remove shell access to unused users.
    - [ ] Setup sessions expiration
    - [ ] Setup password policy
    - [ ] Deactivate cron if not used.
    - [ ] Use unattended-upgrades.
    - [ ] Deactivate kernel modules loading.
    - [ ] Configure sysctl to remove potential attack vectors

### Configuration specific (cloud VPS instance)
    - [ ] Use some tools like debscan and lynis to regulary update the status of your system. 
    - [ ] Use both the firewall of you system and the Cloud provider firewall. Often, your cloud provider gives you an interface for managing firewalling tasks. Use both this interface and the host based firewall of your choice to add an extra layer of security (If your setup is complex and uses many could instances, a combination of both would provide more granularity).

### Level 2 
    - [ ] Configuring the BIOS/UEFI
    - [ ] Activating the UEFI secure boot
    - [ ] Configuring a password on the bootloader
    - [ ] Configuring the memory options
    - [ ] Configuring the kernel options
    - [ ] Disabling kernel modules loading
    - [ ] Configuration option of the Yama LSM
    - [ ] IPv4 configuration options
    - [ ] Disabling IPv6
    - [ ] File System configuration options 
    - [ ] Use specific partitioning options
    - [ ] Configuring a timeout on local user sessions
    - [ ] Ensuring the imputability of administration actions
    - [ ] Disabling the service accounts
    - [ ] Uniqueness and exclusivity of service accounts
    - [ ] Sudo configuration guidelines
    - [ ] Using unprivileged users as target for sudo commands
    - [ ] Banishing the negations in sudo policies
    - [ ] Defining the arguments in sudo specifications
    - [ ] Editing files securely with sudo
    - [ ] Limiting the rights to access sensitive files and directories
    - [ ] Securing access for named sockets and pipes
    - [ ] Dedicating temporary directories to users
    - [ ] Disabling non-essential features of services
    - [ ] Secure remote authentication with PAM
    - [ ] Securing access to remote user databases
    - [ ] Separating the system accounts and directory administrator
    - [ ] Hardening the local messaging service
    - [ ] Configuring aliases for service accounts
    - [ ] Hardening and monitoring the exposed services

### Level 3
    - [ ] Choosing and configuring the hardware
    - [ ] Activating the IOMMU
    - [ ] Access restrictions on /boot
    - [ ] Changing the default value of UMASK
    - [ ] Using Mandatory Access Control features
    - [ ] Creating a group dedicated to the use of sudo
    - [ ] Limiting the number of commands requiring the use of the EXEC directive
    - [ ] Activating AppArmor security profiles
    - [ ] Changing the secrets and access rights as soon as possible
    - [ ] Avoiding using executables with setuid root and setgid root rigths
    - [ ] Using hardened package repositories
    - [ ] Configuring the privileges of the services
    - [ ] Partitioning the services
    - [ ] Implementing a logging system
    - [ ] Implementing dedicated service activity journals
    - [ ] Logging the system activity with auditd
    - [ ] Partitioning the network services



