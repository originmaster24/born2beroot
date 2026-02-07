This project has been created as part of the 42 curriculum by zzhu

Description

A System Administration project focused on deploying a Debian server. The goal is to master virtualization, LVM partitioning, and security hardening (Sudo, SSH, UFW, and password policies).
OS Choice: Debian
Why: I chose for its stability, lightweight footprint (ideal for minimal installs), and the apt package manager's reliability.
Pro/Con: Excellent community support, but slower to adopt "bleeding-edge" software compared to Rocky.

Project Design:
Partitioning: I implemented LVM (Logical Volume Manager). LVM allows me to resize partitions easily and manage multiple disks as a single volume group, which is essential for professional server scalability.
Security: To harden the system, I used libpam-pwquality to enforce a 10-character minimum password length including uppercase, lowercase, and digits. I also configured Sudo to require a TTY, use custom log files for audit trails, and limit password attempts to three. The concerning files are /etc/pam.d/common-password/ and /etc/login.defs.
for ssh hardening I set up /etc/ssh/sshd_config
User Management: I set up a primary user within the user42 group and strictly controlled the root account to ensure all administrative actions are tracked via sudo.

Technical Comparisons

Debian vs. Rocky Linux
I chose Debian because it is a community-driven, "universal" operating system known for its extreme stability and ease of use via the apt package manager. While Rocky Linux is an excellent enterprise-grade choice (designed to be a 1:1 binary compatible alternative to Red Hat Enterprise Linux), it uses the dnf package manager and follows a more corporate-centric structure. Debian felt more appropriate for this project due to its lightweight nature in a minimal install environment.

AppArmor vs. SELinux
For Mandatory Access Control (MAC), Debian uses AppArmor by default, whereas Rocky Linux uses SELinux.
AppArmor is "path-based," meaning it restricts applications based on their file paths. It is generally considered more user-friendly and easier to write profiles for.
SELinux is "label-based" and much more granular. It is powerful but significantly more complex to configure, as every file, process, and user is assigned a security label.

UFW vs. Firewalld
To manage the network firewall, I utilized UFW (Uncomplicated Firewall).
UFW is a simplified interface for iptables that is perfect for standalone servers. It is straightforward: you simply allow or deny specific ports.
Firewalld, the default on Rocky, uses a "zone-based" concept. It is more dynamic and powerful for complex networks where you might move between trusted and untrusted environments, but it has a steeper learning curve for a simple server setup.

VirtualBox vs. UTM
The choice between these two often comes down to your hardware.
VirtualBox is the industry standard for x86 architecture (Intel/AMD). It is a feature-rich Type-2 hypervisor that works across Windows, Linux, and older Macs.
UTM is the go-to for modern Apple Silicon (M1/M2/M3) Macs. It uses the Apple Virtualization Framework and QEMU to provide near-native performance on ARM architecture, which VirtualBox currently struggles to match on those specific chips.

Instructions
SSH Access: Connect from your host machine to the VM using the designated port: ssh <login>@localhost -p 4242
User & Groups: Verify the user is in sudo and user42 groups: getent group sudo user42
Password Policy: Check password aging and expiration rules: chage -l <login>
Hostname & OS: Confirm the correct hostname and Debian version: hostnamectl
Partitioning: Verify the LVM structure and partitions: lsblk
Sudo Configuration: Check sudo version and the existence of the /var/log/sudo/ folder for logs: sudo -V and ls /var/log/sudo/
UFW Firewall: Verify that only Port 4242 is open: sudo ufw status numbered
SSH Service: Confirm SSH is running only on Port 4242: grep "Port" /etc/ssh/sshd_config
Signature Check: Compare the local signature.txt with the disk image (on host): shasum Born2BeRoot.vdi

Monitoring Script
The monitoring script runs automatically every 10 minutes via Cron. /home/zzhu/monitoring.sh

Resources
Debian Security Manual https://www.debian.org/doc/manuals/securing-debian-manual/index.en.html
LVM Guide https://wiki.debian.org/LVM
AI Usage:
Tasks: Clarified awk indexing (1-based), resolved subshell syntax errors in Bash, and troubleshot systemd-timesyncd for clock synchronization.
