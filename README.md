# 🧪 My Zero Trust Architecture (ZTA) Linux Lab

<p align="center">
  <img src="https://github.com/user-attachments/assets/bfcd034f-553d-4a0c-b727-cf0e02f204f7" alt="dNzL2aq">
</p>

### Introduction

I’m doing this lab to test Linux’s power and flexibility by tackling a real-world scenario. Our company, **Redwood Civic Technologies** 🏢, a government contractor, had its California data center taken offline by wildfires 🔥. We had to activate our warm site in Virginia 🌐 and quickly rebuild a critical system with **Zero Trust Architecture (ZTA)** 🔐 to meet government security requirements.

**Government requirements include:**

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


