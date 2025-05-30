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
| **🚨 Assume Breach**            | 📊 System auditing (auditd), 🛡️ SELinux enforcement, 🔥 network firewall       |
| **🌐 Micro-Segmentation**       | 🚧 Restrictive iptables rules allowing only essential traffic             |
| **📈 Continuous Monitoring**    | 📋 Audit rules on key files, ⚙️ process accounting, ⏰ cron-based checks        |
| **👤 User/Posture Enforcement** | 🔒 MFA enforced, 🚪 SSH access limited to authorized disaster recovery users |

### 1. Create Disaster Recovery Users
   
![pznctXd](https://github.com/user-attachments/assets/2b99215b-dea1-4402-aa98-45f4962b641a)

-----

![1rtjxHx](https://github.com/user-attachments/assets/56283ef5-8eb1-4120-90a9-de76340a39fb)

#### Summary:
#### I created three disaster recovery user accounts — drc_jane, drc_mike, and drc_sarah — to represent my recovery team who will securely access the system during an incident.

----

### 2. Create and Assign Groups for Role-Based Access

![M2XoWff](https://github.com/user-attachments/assets/4aba9e65-8eb5-4202-a497-fc21465c0908)

-----

![rwYqfVJ](https://github.com/user-attachments/assets/9e80c4f1-4802-4c31-96a9-f39e68b136a8)

#### Summary:
#### I set up two groups — auditing and recovery — to enforce role-based access control. I assigned Jane and Sarah to auditing for monitoring tasks, and Mike to recovery for restoration duties.

----

### 3. Lock Down SSH to Use Key-Based Authentication Only
   
#### Edit /etc/ssh/sshd_config to include:

![UbOHYfx](https://github.com/user-attachments/assets/b64ba73a-ad59-46f0-b64c-3448e66b1c24)

-----

![0eDhEq9](https://github.com/user-attachments/assets/101eba43-1fb4-4d1d-8987-75751a137a2a)

-----

![YbXMj72](https://github.com/user-attachments/assets/0faa639c-4519-4aca-aa6f-c7d13f46fe2a)

#### Restart SSH:

![wwI8IXF](https://github.com/user-attachments/assets/3bdbdaf7-cfd6-4361-bd41-2d0c7c904fc5)

-----

#### Summary:
#### I locked down SSH to disallow passwords and root login, allowing only my disaster recovery users to connect via SSH keys. This enhances security by preventing unauthorized password access.

----

### 4. Enforce Multi-Factor Authentication (MFA) with Google Authenticator
   
#### Install PAM module:

![L6f3NCH](https://github.com/user-attachments/assets/b14e9927-a2ce-48bd-b82f-013a6f47c425)

-----

#### Edit /etc/pam.d/sshd and add:

![t5Qw5Y3](https://github.com/user-attachments/assets/84bf3eff-82b4-42f2-bfc6-8fc221532524)

-----

#### Confirm ChallengeResponseAuthentication yes in /etc/ssh/sshd_config and restart SSH.

![aTolmpB](https://github.com/user-attachments/assets/3e2915d6-70ba-42af-8521-88f37c286c37)

-----

#### Each user runs to set up MFA.

![t4aJtY5](https://github.com/user-attachments/assets/09e6a6b9-7f8c-4a57-bb52-c5b74623f168)

-----

#### Summary:
#### I enabled MFA for SSH access using Google Authenticator. This requires users to provide a time-based code in addition to their SSH key, adding a strong second layer of identity verification.

----

### 5. Configure Sudo Access Based on Roles
   
#### Edit sudoers with sudo visudo to add:

![ULJ4wY0](https://github.com/user-attachments/assets/df977c0d-27df-4e7f-932d-325a7cafc27e)

-----
#### Summary:
#### I tailored sudo permissions to follow least-privilege principles. Jane and Sarah get limited commands for auditing, while Mike has full sudo rights for recovery tasks.

----

### 6. Enable and Configure Auditd for Monitoring Critical Files

![KaexAjO](https://github.com/user-attachments/assets/a02e04bf-e03e-4d96-8643-901206e596d6)

-----

![cRjI2CT](https://github.com/user-attachments/assets/2de3a030-1144-4f77-a8fa-60b898153402)

-----

#### Summary:
#### I installed auditd to continuously monitor critical files like /etc/passwd and /etc/sudoers, so I can detect any unauthorized changes in real time.

----

### 7. Enforce Mandatory Access Controls with SELinux

![6DSoEWu](https://github.com/user-attachments/assets/3a2fbcae-8ebd-4176-876d-6b2876bb6925)

-----

#### Summary:
#### I enabled SELinux to enforce mandatory access controls on critical system services like SSH, restricting their capabilities to reduce risk from potential exploits.

----

### 8. Restrict Network Traffic with Iptables
   
#### Flush rules and set default policy to DROP:

![l9tvVFc](https://github.com/user-attachments/assets/9ecaa589-c7b7-4eef-87e6-59ec826b044c)

-----

#### Allow essential traffic:

![23gopOV](https://github.com/user-attachments/assets/49a72fa3-bbe9-442e-9ad5-39de31815210)

-----

![mopr4On](https://github.com/user-attachments/assets/8406a14e-693e-46dd-b4ad-13519f88a4da)

#### Summary:
#### I set strict firewall rules allowing only essential traffic: SSH, DNS, HTTP, and HTTPS. All other connections are blocked by default, enforcing micro-segmentation and limiting network exposure.

----

### 9. Enable Command and System Activity Logging

![cAzpZtv](https://github.com/user-attachments/assets/eaa80d94-af19-4e6c-8abd-ad4aba452a7b)

-----



#### Summary:
#### I installed process accounting tools to log all command executions and system activities. This helps me continuously monitor user behavior and system events.

----

### 10. Create Dynamic Security Response Script

#### Create /usr/local/bin/monitor_passwd.sh with the following:

#### Make executable:

#### Add to cron (run every 5 minutes):

![cAzpZtv](https://github.com/user-attachments/assets/b9820590-3ab9-4717-ab18-794b6938f2a3)

-----

#### Add line:

![AvurrZH](https://github.com/user-attachments/assets/846f5964-61dc-42e4-a9a3-0fec8cf3eca8)

-----

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
- **SELinux**: Mandatory Access Control (MAC) for key applications such as SSH daemon.
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
- **CentOS** 
