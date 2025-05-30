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

This lab lets me:

- 🖥️ Build a hardened Ubuntu server  
- 🏗️ Create a ZTA-compliant environment  
- 👥 Onboard disaster recovery users securely  

| **Zero Trust Tenet**         | **Linux Lab Implementation Summary**                                   |
| ---------------------------- | ---------------------------------------------------------------------- |
| **🔍 Verify Explicitly**        | 🔐 SSH key-only access, 🛡️ MFA with Google Authenticator, 📜 audit logs         |
| **🔑 Least Privilege Access**   | 👥 Role-based groups, 🔒 limited sudo permissions for users                  |
| **🚨 Assume Breach**            | 📊 System auditing (auditd), 🛡️ AppArmor enforcement, 🔥 network firewall       |
| **🌐 Micro-Segmentation**       | 🚧 Restrictive iptables rules allowing only essential traffic             |
| **📈 Continuous Monitoring**    | 📋 Audit rules on key files, ⚙️ process accounting, ⏰ cron-based checks        |
| **👤 User/Posture Enforcement** | 🔒 MFA enforced, 🚪 SSH access limited to authorized disaster recovery users |


