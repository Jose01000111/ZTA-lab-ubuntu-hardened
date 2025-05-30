# 🧪 My Zero Trust Architecture (ZTA) Linux Lab

<p align="center">
  <img src="https://github.com/user-attachments/assets/bfcd034f-553d-4a0c-b727-cf0e02f204f7" alt="dNzL2aq">
</p>

### Introduction

I’m doing this lab to test Linux’s power and flexibility by tackling a real-world scenario. Our company, **Redwood Civic Technologies** 🏢, a government contractor, had its California data center taken offline by wildfires 🔥. We had to activate our warm site in Virginia 🌐 and quickly rebuild a critical system with **Zero Trust Architecture (ZTA)** 🔐 to meet government security requirements.

### **Government requirements include:**

- 🔒 Strict Zero Trust Architecture (ZTA)  
- 🛡️ Least-privilege access  
- 🔑 Strong identity verification
  
### Mapping Zero Trust Tenets to Linux Implementation

To bring Zero Trust Architecture (ZTA) to life on a Linux Ubuntu server, I’ve mapped each core ZTA principle to specific Linux-based security controls and practices. This chart summarizes how the lab’s Linux configurations and tools address the essential tenets of Zero Trust, helping ensure a secure, compliant disaster recovery environment.
 

| **Zero Trust Tenet**         | **Linux Lab Implementation Summary**                                   |
| ---------------------------- | ---------------------------------------------------------------------- |
| **🔍 Verify Explicitly**        | 🔐 SSH key-only access, 🛡️ MFA with Google Authenticator, 📜 audit logs         |
| **🔑 Least Privilege Access**   | 👥 Role-based groups, 🔒 limited sudo permissions for users                  |
| **🚨 Assume Breach**            | 📊 System auditing (auditd), 🛡️ AppArmor enforcement, 🔥 network firewall       |
| **🌐 Micro-Segmentation**       | 🚧 Restrictive iptables rules allowing only essential traffic             |
| **📈 Continuous Monitoring**    | 📋 Audit rules on key files, ⚙️ process accounting, ⏰ cron-based checks        |
| **👤 User/Posture Enforcement** | 🔒 MFA enforced, 🚪 SSH access limited to authorized disaster recovery users |

### 1. Create Disaster Recovery Users
   
### 📷 Screenshot Placeholder

#### Summary:
#### I created three disaster recovery user accounts — drc_jane, drc_mike, and drc_sarah — to represent my recovery team who will securely access the system during an incident.

----

### 2. Create and Assign Groups for Role-Based Access

### 📷 Screenshot Placeholder

#### Summary:
#### I set up two groups — auditing and recovery — to enforce role-based access control. I assigned Jane and Sarah to auditing for monitoring tasks, and Mike to recovery for restoration duties.

----

### 3. Lock Down SSH to Use Key-Based Authentication Only
   
#### Edit /etc/ssh/sshd_config to include:

### 📷 Screenshot Placeholder

#### Restart SSH:

### 📷 Screenshot Placeholder

#### Manually set up SSH keys in each user’s ~/.ssh/authorized_keys.

#### Summary:
#### I locked down SSH to disallow passwords and root login, allowing only my disaster recovery users to connect via SSH keys. This enhances security by preventing unauthorized password access.

----

### 4. Enforce Multi-Factor Authentication (MFA) with Google Authenticator
   
#### Install PAM module:

### 📷 Screenshot Placeholder

#### Edit /etc/pam.d/sshd and add:

### 📷 Screenshot Placeholder

#### Confirm ChallengeResponseAuthentication yes in /etc/ssh/sshd_config and restart SSH.

#### Each user runs to set up MFA.

### 📷 Screenshot Placeholder

#### Summary:
#### I enabled MFA for SSH access using Google Authenticator. This requires users to provide a time-based code in addition to their SSH key, adding a strong second layer of identity verification.

----

### 5. Configure Sudo Access Based on Roles
   
#### Edit sudoers with sudo visudo to add:

### 📷 Screenshot Placeholder

#### Summary:
#### I tailored sudo permissions to follow least-privilege principles. Jane and Sarah get limited commands for auditing, while Mike has full sudo rights for recovery tasks.

----

### 6. Enable and Configure Auditd for Monitoring Critical Files

### 📷 Screenshot Placeholder

#### Summary:
#### I installed auditd to continuously monitor critical files like /etc/passwd and /etc/sudoers, so I can detect any unauthorized changes in real time.

----

### 7. Enforce Mandatory Access Controls with AppArmor

### 📷 Screenshot Placeholder

#### Summary:
#### I enabled AppArmor to enforce mandatory access controls on critical system services like SSH, restricting their capabilities to reduce risk from potential exploits.

----

### 8. Restrict Network Traffic with Iptables
   
#### Flush rules and set default policy to DROP:

### 📷 Screenshot Placeholder

#### Allow essential traffic:

### 📷 Screenshot Placeholder

#### Save rules:

### 📷 Screenshot Placeholder

#### Summary:
#### I set strict firewall rules allowing only essential traffic: SSH, DNS, HTTP, and HTTPS. All other connections are blocked by default, enforcing micro-segmentation and limiting network exposure.

----

### 9. Enable Command and System Activity Logging

### 📷 Screenshot Placeholder

#### Summary:
#### I installed process accounting tools to log all command executions and system activities. This helps me continuously monitor user behavior and system events.

----

### 10. Create Dynamic Security Response Script

#### Create /usr/local/bin/monitor_passwd.sh with the following:

### 📷 Screenshot Placeholder

#### Make executable:

### 📷 Screenshot Placeholder

#### Add to cron (run every 5 minutes):

### 📷 Screenshot Placeholder

#### Add line:

### 📷 Screenshot Placeholder

#### Summary:
#### I automated monitoring of critical file changes and scripted an immediate lockdown of recovery user accounts if /etc/passwd is modified, proactively protecting against potential breaches.

----

# 🛠️ Disaster Recovery Lab – Tech Stack

## 🔐 Authentication & Access Control
- **OpenSSH**: Hardened for key-based authentication only; password logins disabled.
- **Google Authenticator (`libpam-google-authenticator`)**: Provides time-based one-time password (TOTP) for MFA.
- **PAM (Pluggable Authentication Modules)**: Integrated with Google Authenticator for MFA enforcement.
- **User & Group Management**: Role-based access control using `adduser`, `groupadd`, and `usermod`.

## 🧑‍💻 Privilege Management
- **Sudoers Configuration (`visudo`)**: Fine-grained sudo access configured per role.
- **Least Privilege Enforcement**: Users can only run specific commands relevant to their roles.

## 🔍 Monitoring & Auditing
- **Auditd + Audispd**: Monitors changes to critical system files like `/etc/passwd` and `/etc/sudoers`.
- **GNU acct (`lastcomm`)**: Tracks all executed commands by users for accountability.
- **Journalctl**: System log viewer for reviewing real-time system events and alerts.

## 🧱 System Hardening
- **AppArmor**: Mandatory Access Control (MAC) for key applications such as SSH daemon.
- **SSH Configuration (`sshd_config`)**:
  - PasswordAuthentication disabled
  - Root login disabled
  - Limited to authorized users only

## 🔥 Firewall & Network Control
- **iptables**:
  - Default deny-all policy (DROP)
  - Allows only essential traffic (SSH, DNS, HTTP/S)
- **iptables-persistent**: Ensures firewall rules persist across reboots.

## ⚙️ Automation & Response
- **Custom Script (`/usr/local/bin/monitor_passwd.sh`)**:
  - Monitors `/etc/passwd` for unauthorized changes.
  - Automatically locks DR users if modification is detected.
- **Cron Job**:
  - Runs the security script every 5 minutes to maintain a responsive security posture.

## 🐧 Operating System
- **Ubuntu Linux** (Debian-based distribution)
